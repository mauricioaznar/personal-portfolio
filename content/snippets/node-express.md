---
title: Nodejs (express)
---

## Introduction 

Node.js is an open-source, cross-platform, back-end JavaScript runtime
environment that runs on the V8 engine and executes JavaScript
code outside a web browser. ([nodejs docs](https://nodejs.org/en/docs/))


<br />

## Set up

index.js
```javascript
const express = require('express')
const mongoose = require('mongoose')
const cookieSession = require('cookie-session')
const passport = require('passport')
const bodyParser = require('body-parser')
const keys = require('./config/keys')
require('./models/User')
require('./models/Survey')
require('./services/passport')

mongoose.connect(keys.mongoURI)


const app = express()

app.use(bodyParser.json())
app.use(
  cookieSession({
    maxAge: 30 * 24 * 60 * 60 * 1000,
    keys: [keys.cookieKey]
  })
)
app.use(passport.initialize())
app.use(passport.session())

require('./routes/authRoutes')(app)
require('./routes/billingRoutes')(app)
require('./routes/surveyRoutes')(app)

if (process.env.NODE_ENV === 'production') {
  // Express will serve up production assets
  // like our main.js file, or main.css file!
  app.use(express.static('client/build'))

  // Express will serve up the index.html file
  // if it doesnt recognize the route
  const path = require('path')
  app.get('*', (req, res) => {
    res.sendFile(path.resolve(__dirname, 'client', 'build', 'index.html'))
  })
}

const port = process.env.PORT || 5000


app.listen(port, () => {
  console.log(`App is listening on port ${port}`)
})
```

<br />

For usage of cors  use the following:

```javascript
const cors = require('cors')

app.use(cors())
```


<br />

## Middlewares
Express knows that a function with 3 arguments is treated as a middleware

require-credit.js
```javascript
module.exports = (req, res, next) => {
  if (req.user.credits < 1) {
    return res.status(403).send({ error: 'Not enough credits!'})
  }
  next()
}
```

require-login.js
```javascript
module.exports = (req, res, next) => {
  if (!req.user) {
    return res.status(401).send({ error: 'You must log in!'})
  }
  next()
}
```

<br />

## Mongoose

Recipient.js
```javascript
const mongoose = require('mongoose')
const {Schema} = mongoose

const recipientSchema = new Schema({
  email: String,
  responded: {
    type: Boolean,
    default: false
  }
})

module.exports = recipientSchema
```

<br />

Survey.js
```javascript
const mongoose = require('mongoose')
const {Schema} = mongoose
const RecipientSchema = require('./Recipient')

const surveySchema = new Schema({
  title: String,
  body: String,
  subject: String,
  recipients: [RecipientSchema],
  yes: {
    type: Number,
    default: 0
  },
  no: {
    type: Number,
    default: 0
  },
  _user: {
    type: Schema.Types.ObjectId,
    ref: 'User'
  },
  dateSent: Date,
  lastResponded: Date
})

mongoose.model('surveys', surveySchema)
```

<br />

User.js
```javascript
const mongoose = require('mongoose')
const {Schema} = mongoose

const userSchema = new Schema({
  googleId: String,
  credits: {
    type: Number,
    default: 0
  }
})

mongoose.model('users', userSchema)
```

<br />

## Routes

survey-route.js
````javascript
const _ = require('lodash')
const {Path} = require('path-parser')
const {URL} = require('url')
const mongoose = require('mongoose')
const requireLogin = require('../middlewares/requireLogin')
const requireCredits = require('../middlewares/requireCredits')
const Mailer = require('../services/Mailer')
const surveyTemplate = require('../services/emailTemplates/emailTemplate')

const Survey = mongoose.model('surveys')

module.exports = app => {


  app.get('/api/surveys',
    requireLogin,
    async (req, res) => {
      const surveys = await Survey
        .find({
          _user: req.user.id
        })
        .select({
          recipients: false
        })
      res.send(surveys)
    }
  )

  app.get('/api/surveys/:surveyId/:choice', (req, res) => {
    res.send('Thanks for voting')
  })


  // remember use ngrok and change url in sendgrid
  app.post('/api/surveys/webhooks', (req, res) => {
    const p = new Path('/api/surveys/:surveyId/:choice')

    _.chain(req.body)
      .map((event) => {
        const url = new URL(event.url)
        const match = p.test(url.pathname)
        if (match) {
          return {
            email: event.email,
            surveyId: match.surveyId,
            choice: match.choice
          }
        }
      })
      .compact()
      .uniqBy('email', 'surveyId')
      .each(({surveyId, email, choice}) => {
        Survey.updateOne({
          _id: surveyId,
          recipients: {
            $elemMatch: { email: email, responded: false}
          }
        }, {
          $inc: { [choice]: 1},
          $set: { 'recipients.$.responded': true },
          lastResponded: new Date()
        }).exec()
      })
      .value()





    res.send({})
  })

  app.post(
    '/api/surveys',
    requireLogin,
    requireCredits,
    async (req, res) => {
      try {
        const {title, subject, body, recipients} = req.body
        const survey = new Survey({
          title,
          subject,
          body,
          recipients: recipients.split(',').map(email => ({ email: email.trim() }) ),
          _user: req.user.id,
          dateSent: Date.now()
        })

        // Great place to send an email
        const mailer = new Mailer(survey, surveyTemplate(survey))
        await mailer.send()
        await survey.save()
        req.user.credits -= 1
        const user = await req.user.save()
        res.send(user)
      } catch (e) {
        res.status(422).send(e)
      }
    }
  )
}
````

<br />


billing-route.js
```javascript
const keys = require('../config/keys')
const stripe = require('stripe')(keys.stripeSecretKey)
const requireLogin = require('../middlewares/requireLogin')

module.exports = (app) => {
  app.post(
    '/api/stripe',
    requireLogin,
    async (req, res) => {
      const charge = await stripe.charges.create({
        amount: 500,
        currency: 'usd',
        description: '$5 for 5 credits',
        source: req.body.id
      })

      req.user.credits += 5

      const user = await req.user.save()

      res.send(user)
    }
  )
}
```

<br />

auth-route.js
```javascript
const passport = require('passport')

module.exports = (app) => {
  app.get('/auth/google', passport.authenticate(
    'google',
    {
      scope: ['profile', 'email']
    }, () => {

    })
  )

  app.get(
    '/auth/google/callback',
    passport.authenticate('google'),
    (req, res) => {
      res.redirect('/surveys')
    }
  )

  app.get('/api/logout', (req, res) => {
    req.logout()
    res.redirect('/')
  })

  app.get('/api/current_user', (req, res) => {
    res.send(req.user)
  })
}
```

<br />

## Config

### keys (.js files)
We can set up different keys depending on the environment the app is running (dev/prod)

prod.js
```javascript
// prod.js - production keys

module.exports = {
  googleClientID: process.env.GOOGLE_CLIENT_ID,
  googleClientSecret: process.env.GOOGLE_CLIENT_SECRET,
  mongoURI: process.env.MONGO_URI,
  cookieKey: process.env.COOKIE_KEY,
  stripePublishableKey: process.env.STRIPE_PUBLISHABLE_KEY,
  stripeSecretKey: process.env.STRIPE_SECRET_KEY,
  sendGridApiKey: process.env.SEND_GRID_KEY,
  redirectDomain: process.env.REDIRECT_DOMAIN
}
```

<br />

dev.js
```javascript
// dev.js - dont commit this

module.exports = {
  googleClientID: '...',
  googleClientSecret: '...',
  mongoURI: '...',
  cookieKey: 'asdfasdfasdfasdfahsdfhllkjdflkahslkdfhlakshdflkhalkh',
  stripePublishableKey: '...',
  stripeSecretKey: '...',
  sendGridApiKey: '...',
  redirectDomain: 'http://localhost:3000/'
}
```

<br />

Depending on the `process.env.NODE_ENV` value, we decide which file to load.
keys.js
```javascript
// key.js figure out what set of credentials to return

if (process.env.NODE_ENV === 'production') {
  module.exports = require('./prod')
} else {
  module.exports = require('./dev')
}

```
