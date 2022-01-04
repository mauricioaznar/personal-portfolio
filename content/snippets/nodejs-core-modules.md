---
title: Nodejs core modules
---

## Introduction 

Node.js is an open-source, cross-platform, back-end JavaScript runtime
environment that runs on the V8 engine and executes JavaScript
code outside a web browser. ([nodejs docs](https://nodejs.org/en/docs/))


<br />

## Set up

index.js

```javascript
const express = require('content/snippets/nodejs-express')
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


---
title: Nodejs techniques
---

## Introduction

## Handlebars
*andrew mead -node js*

Handlebars is a simple templating language. ([handlebars docs](https://handlebarsjs.com/guide/))

It uses a template and an input object to
generate HTML or other text formats. Handlebars templates look like regular text with embedded Handlebars expressions.

`<p>{{firstname}} {{lastname}}</p>`

A handlebars expression is a {{, some contents, followed by a }}.
When the template is executed, these expressions are replaced with
values from an input object.


<br />

### Usage

app.js

```javascript
const path = require('path')
const express = require('content/snippets/nodejs-express')
const hbs = require('hbs')
const geocode = require('./geocode')
const forecast = require('./forecast')

const app = express()
const port = process.env.PORT || 3000

// Define path for Express config -
const publicDirectoryPath = path.join(__dirname, '../public')
const viewsDirectoryPath = path.join(__dirname, '../templates/views')
const partialsDirectoryPath = path.join(__dirname, '../templates/partials')

// Setup handlebars engine and views location
app.set('view engine', 'hbs')
app.set('views', viewsDirectoryPath)
hbs.registerPartials(partialsDirectoryPath)

// Setup static directory to serve
app.use(express.static(publicDirectoryPath))

app.get('', (req, res) => {
  res.render('index', {
    title: 'Weather App',
    name: 'Andrew Mead'
  })
})

app.get('/help', (req, res) => {
  res.render('help', {
    title: 'Help',
    name: 'Andrew Mead'
  })
})

app.get('/about', (req, res) => {
  res.render('about', {
    title: 'About',
    name: 'Andrew Mead'
  })
})

app.get('/weather', (req, res) => {
  if (!req.query.address) {
    return res.send({
      error: 'You must provide an address'
    })
  }

  const address = req.query.address

  geocode(address, (err, geoData) => {
    if (err) {
      res.send(err)
      return
    }
    const { latitude, longitude } = geoData
    forecast(latitude, longitude, (err, forecastData) => {
      if (err) {
        res.send(err)
        return
      }
      res.send({ forecastData, geoData })
      console.log(forecastData)
    })
  })

})

app.get('/products', (req, res) => {
  if (!req.query.search) {
    return res.send({
      error: 'You must provide a search term'
    })
  }
  res.send({ somedata: '' })
})

app.get('*', (req, res) => {
  res.send('My 404 page')
})

app.listen(port, () => {
  console.log('Server is up on port 3000 ' + port)
})
```

<br />

### Partials

./templates/partials

footer.hbs

```html
<footer>
    <p>Created by {{name}}</p>
</footer>
```

header.hbs

```html
<h1>{{title}}</h1>

<div>
  <a href="/">Weather</a>
  <a href="/about">About</a>
  <a href="/help">Help</a>
</div>
```

<br />

### Views

./templates/views

404.hbs
```html
<!DOCTYPE html>

<html>

<head>
    <title>404</title>
    <link rel="icon" href="/img/weather.png">
    <link rel="stylesheet" href="/css/styles.css">
</head>

<body>
    <div class="main-content">
        {{>header}}
        <p>{{errorMessage}}</p>
    </div>
    
    {{>footer}}
</body>

</html>
```

about.hbs
```html
<!DOCTYPE html>

<html>

<head>
    <title>About</title>
    <link rel="icon" href="/img/weather.png">
    <link rel="stylesheet" href="/css/styles.css">
</head>

<body>
    <div class="main-content">
        {{>header}}
    </div>
    <img class="portrait" src="/img/me.png">
    
    {{>footer}}
</body>

</html>
```

help.hbs
```html
<!DOCTYPE html>

<html>

<head>
    <title>Help</title>
    <link rel="icon" href="/img/weather.png">
    <link rel="stylesheet" href="/css/styles.css">
</head>

<body>
    <div class="main-content">
        {{>header}}
        <p>{{helpText}}</p>
    </div>
    
    {{>footer}}
</body>

</html>
```

index.hbs
```html
<!DOCTYPE html>

<html>

<head>
    <title>Weather</title>
    <link rel="icon" href="/img/weather.png">
    <link rel="stylesheet" href="/css/styles.css">
</head>

<body>
    <div class="main-content">
        {{>header}}

        <p>Use this site to get your weather!</p>
        <form>
            <input placeholder="location" />
            <button>Search</button>
        </form>
        <p id="message-1"></p>
        <p id="message-2"></p>
    </div>

    {{>footer}}

    <script src="/js/app.js"></script>
</body>

</html>
```
<br />

### Public
./public/js

File executed on the client

app.js
```javascript
const weatherFrom = document.querySelector('form')
const search = document.querySelector('input')
const messageOne = document.querySelector('#message-1')
const messageTwo = document.querySelector('#message-2')

weatherFrom.addEventListener('submit', (e) => {
  e.preventDefault()

  const location = search.value

  messageOne.textContent = 'Loading...'
  messageTwo.textContent = ''

  fetch('/weather?address=' + location)
    .then(response => {
      response.json().then(data => {
        if (data.error) {
          messageOne.textContent = data.error
          messageTwo.textContent = ''
        } else {
          messageOne.textContent = data.geoData.place_name
          messageTwo.textContent = `The forecast for ${data.geoData.place_name} is ${data.forecastData.temperature}`
        }
      })
    })

})
```


### Functions used in Usage

forecast.js
```javascript
const axios = require('axios')

const forecast = (latitude, longitude, callback) => {
  const urlLatitude = latitude < 0 ? encodeURIComponent("'" + latitude + "'") : latitude
  const urlLongitude = longitude < 0 ? encodeURIComponent("'" + longitude + "'") : longitude
  const weatherurl = 'http://api.weatherstack.com/current?access_key=035ba6b559a56683c60ec1fb92d85fa5&query=' + urlLatitude + ',' + urlLongitude

  console.log(weatherurl)

  axios.get(weatherurl)
    .then(result => {
      if (result.data.error) {
        callback('Unable to find location, try another search.', undefined)
      } else {
        callback(undefined, result.data.current)
      }
    })
    .catch(error => {
      callback(error, undefined)
    })


}

module.exports = forecast
```

geocode.js
```javascript
const axios = require('axios')

const geocode = (address, callback) => {
  const url = 'https://api.mapbox.com/geocoding/v5/mapbox.places/' + encodeURIComponent(address) + '.json?access_token=pk.eyJ1IjoibWFndWFzdHVwYWd1YXMiLCJhIjoiY2tpdGxoc2N3MDNjdTMzbnVxaWd0NHd1YSJ9.zFx8KtqgRhfgIiQ1VkEsqg'

  axios.get(url)
    .then(result => {
      if (result.data.features.length === 0) {
        callback('Unable to find location!. Try another search.', undefined)
      } else {
        const [latitude, longitude] = result.data.features[0].center
        const {place_name} = result.data.features[0]
        callback(undefined, {
          latitude,
          longitude,
          place_name
        })
      }
    })
    .catch(error => {
      callback('Unable to connect to location services!', undefined)
    })

}

module.exports = geocode
```

<br />



## Sendgrid
dependencies used for this script `"@sendgrid/mail": "^7.4.2"` & `"sendgrid": "^5.2.3",`

Mailer.js
```javascript
const sgMail = require("@sendgrid/mail");
const keys = require("../config/keys");

class Mailer {
  constructor({ subject, recipients }, content) {
    sgMail.setApiKey(keys.sendGridApiKey);
    this.msg = {
      to: recipients.map(({ email }) => email),
      from: "email@gmail.com",
      subject: subject,
      html: content,
      trackingSettings: { enable_text: true, enabled: true }
    };
  }

  async send() {
    return await sgMail.send(this.msg);
  }
}

module.exports = Mailer;
```

<br />

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

email-template.js
```javascript
const keys = require('../../config/keys')

module.exports = (survey) => {
  return `
    <html>
      <body>
        <div>
          <h3>I'd like your input!</h3>
          <p>Please answer the following question</p>
          <p>${survey.body}</p>
          <div>
            <a href="${keys.redirectDomain}api/surveys/${survey.id}/yes">Yes</a>
          </div>
          <div>
            <a href="${keys.redirectDomain}api/surveys/${survey.id}/no">No</a>
          </div>
        </div>
      </body>
    </html>
  `
}
```

<br />


send-email.js
```javascript
const sgMail = require('@sendgrid/mail')

const sendGridApiKey = process.env.SENDGRID_API_KEY

sgMail.setApiKey(sendGridApiKey)

const sendWelcomeEmail = (email, name) => {
    sgMail.send({
        to: email,
        from: 'mauricioaznar94@gmail.com',
        subject: 'Welcome to the app',
        text: `Welcome to the app, ${name}. Let me know how you get along with the app.`
    })
}

module.exports = {
    sendWelcomeEmail
}
```

## Passport
dependencies used for this script `"passport": "^0.4.1"` & `"passport-google-oauth20": "^2.0.0",`

require passport.js files in app.js `require('./passport.js')`


passport.js
```javascript
const passport = require('passport')
const GoogleStrategy = require('passport-google-oauth20').Strategy
const keys = require('../config/keys')
const mongoose = require('mongoose')

const User = mongoose.model('users')

passport.serializeUser((user, done) => {
  done(null, user.id)
})

passport.deserializeUser(async (id, done) => {
  const user = await User.findById(id)
  done(null, user)
})

// proxy is true so that googlestrategy trust proxies and enables https (avoid heroku proxy issue)
passport.use('google', new GoogleStrategy({
    clientID: keys.googleClientID,
    clientSecret: keys.googleClientSecret,
    callbackURL: '/auth/google/callback',
    proxy: true
  }, async (accessToken, refreshToken, profile, done) => {
    const existingUser = await User.findOne({googleId: profile.id})
    if (existingUser) {
      // we already have a record with the given profile ID
      done(null, existingUser)
      return
    }
    // we dont have a user record with this id, make a new record
    const newUser = await new User({googleId: profile.id}).save()
    done(null, newUser)
  })
)
```

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


passport.js

```javascript
const passport = require('passport')
const User = require('../models/user')
const config = require('../config')
const JwtStrategy = require('passport-jwt').Strategy
const ExtractJwt = require('passport-jwt').ExtractJwt
const LocalStrategy = require('passport-local')
const bcrypt = require('bcryptjs')

// Create local strategy
const localOptions = {usernameField: 'email'}
const localLogin = new LocalStrategy(localOptions, function (email, password, done) {
  // Verify this username and password, call done with the user
  // if it is the correct username and password
  // otherwise, call done with token
  User.findOne({email:email}, function (err, user) {
    if (err) { return done(err) }
    if (!user) { return done(null, false) }

    // compare password - is 'password' equal to user.password?
    user.comparePassword(password, function (err, isValid) {
      if (err) { return done(err) }
      if (!isValid) { return done(null, false)}
      return done(null, user)
    })
  })
})


// Setup options for JWT Strategy
const jwtOptions = {
  jwtFromRequest: ExtractJwt.fromHeader('authorization'),
  secretOrKey: config.secret
}

// Create JWT strategy
const jwtLogin = new JwtStrategy(jwtOptions, function (payload, done) {
  // See if the user ID in the payload exists in our database
  // If it does, call 'done' with that other
  // otherwise, call without a user object

  User.findById(payload.sub, function (err, user)  {
    if (err) {
      return done(err, false)
    }
    if (user) {
      done(null, user)
    } else {
      done(null, false)
    }
  })
})

passport.use(jwtLogin)
passport.use(localLogin)
```

<br />

User.js

```javascript
const mongoose = require('mongoose')
const bcrypt = require('bcryptjs')
const Schema = mongoose.Schema;

// Define our model
const userSchema = new Schema({
  email: {
    type: String,
    unique: true,
    lowercase: true
  },
  password: String
})

userSchema.methods.comparePassword = function(candidatePassword, callback) {
  bcrypt.compare(candidatePassword, this.password, (err, isValid) => {
    if (err) { return callback(err) }
    console.log(isValid)
    callback(null, isValid)
  })
}

// Hash the plain text password before saving
userSchema.pre('save', async function (next) {
  const user = this
  try {
    if (user.isModified('password')) {
      user.password = await bcrypt.hash(user.password, 8, null)
    }
  } catch (e) {
    console.log(e)
  }

  next()
})


// Create the model class
const ModelClass = mongoose.model('user', userSchema)

// Export the model
module.exports = ModelClass
```

<br />

## Stripe.js
Dependency used `"stripe": "^8.137.0"`

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

## Middlewares

### User verification and insertions in request Object

auth.js
```javascript
const jwt = require('jsonwebtoken')
const User = require('../models/user')

const auth = async (req, res, next) => {
  try {
    // token gets sent through headers and has to be extracted
    const token = req.header('Authorization').replace('Bearer ', '')
    const decoded = jwt.verify(token, process.env.JWT_SECRET)
    const user = await User.findOne({_id: decoded._id, 'tokens.token': token})

    if (!user) {
      throw new Error()
    }
    //user and token object get merged into the request object
    req.token = token
    req.user = user
  } catch (e) {
    res.status(401).send({error: 'Please authenticate.'})
  }
  // function called to signal express that it can continue processing the request
  next()
}

module.exports = auth
```


## Routing

### Pagination route

GET /tasks?completed=true

GET /tasks?limit=2&skip=1

GET /tasks?sortBy=createdAt:desc

task.js
```javascript
router.get('/tasks', authMiddleware, async (req, res) => {
  const match = {}
  const sort = {}
  if (req.query.completed) {
    match.completed = req.query.completed === 'true'
  }
  if (req.query.sortBy) {
    const parts = req.query.sortBy.split(':')
    sort[parts[0]] = parts[1] === 'asc' ? 1 : -1
  }
  try {
    const user = await User.findOne({_id: req.user._id})
      .populate({
        path: 'tasks',
        match,
        options: {
          limit: parseInt(req.query.limit),
          skip: parseInt(req.query.skip),
          sort
        }
      })
    // const tasks = await Task.find({owner: req.user._id})
    return res.send(user.tasks)
  } catch (error) {
    return res.status(500).send()
  }
})
```


## Socket.io

### Server

app.js

```javascript
const express = require('content/snippets/nodejs-express')
const http = require('http')
const path = require('path')
const socketio = require('socket.io')
const Filter = require('bad-words')

const { generateMessage, generateLocationMessage } = require('./utils/messages')
const { addUser, removeUser, getUsersInRoom, getUser } = require('./utils/users')

const app = express()
const server = http.createServer(app)
const io = socketio(server)

app.use(express.static(path.join(__dirname, '../', 'public')))

let count = 0

io.on('connection', (socket) => {

  socket.on('join', ({ username, room }, callback) => {
    const { error, user } = addUser({ id: socket.id, username, room })

    if (error) {
      return callback(error)
    }

    socket.join(user.room)

    socket.emit('message', generateMessage('Admin', 'Welcome!'))
    socket.broadcast.to(user.room).emit('message', generateMessage('Admin', `${user.username} has joined`))
    io.to(user.room).emit('roomData', {
      room: user.room,
      users: getUsersInRoom(user.room)
    })

    callback()
  })


  socket.on('sendMessage', (message, callback) => {

    const user = getUser(socket.id)

    const filter = new Filter()
    if (filter.isProfane(message)) {
      return callback('Profanity is not allowed')
    }

    io.to(user.room).emit('message', generateMessage(user.username, message))
    callback()
  })
  socket.on('disconnect', () => {
    const user = removeUser(socket.id)

    if (user) {
      io.to(user.room).emit('message', generateMessage('Admin', `${user.username} has left`))
      io.to(user.room).emit('roomData', {
        room: user.room,
        users: getUsersInRoom(user.room)
      })
    }
  })

  socket.on('sendLocation', (coordinates, callback) => {
    const user = getUser(socket.id)

    io.to(user.room).emit('locationMessage', generateLocationMessage(user.username, coordinates))
    callback()
  })
})

const port = process.env.PORT || 3000

server.listen(port, () => {
  console.log('App is running on port ' + port)
})
```

./utils/users.js
```javascript
const users = []

// addUser, removeUser, getUser, getUsersInRoom

const addUser = ({ id, username, room }) => {
    // Clean the data
    username = username.trim().toLowerCase()
    room = room.trim().toLowerCase()

    // Validate the data
    if (!username || !room) {
        return {
            error: 'Username and room are required!'
        }
    }

    // Check for existing user
    const existingUser = users.find((user) => {
        return user.room === room && user.username === username
    })

    // Validate username
    if (existingUser) {
        return {
            error: 'Username is in use!'
        }
    }

    // Store user
    const user = {id, username, room}
    users.push(user)
    return { user }
}

const removeUser = (id) => {
    const index = users.findIndex(user => {
        return id === user.id
    })

    if (index !== -1) {
        return users.splice(index, 1)[0]
    }
}

const getUser = (id) => {
    return users.find(user => {
        return user.id === id
    })
}

const getUsersInRoom = (roomName) => {
    return users.filter(user => {
        return user.room === roomName.toLowerCase()
    })
}

module.exports = {
    addUser,
    removeUser,
    getUser,
    getUsersInRoom
}
```

```javascript
const generateMessage = (username, text) => {
    return {
        username,
        text,
        createdAt: new Date().getTime()
    }
}

const generateLocationMessage = (username, url) => {
    return {
        username,
        url,
        createdAt: new Date().getTime()
    }
}
module.exports = {
    generateMessage,
    generateLocationMessage
}
```

### Client

client.js
```javascript
const socket = io()

// Elements
const $messageForm = document.querySelector('#message-form')
const $input = document.querySelector('#input')
const $messageFormButton = document.querySelector('#increment')
const $sendLocationButton = document.querySelector('#send-location')
const $messages = document.querySelector('#messages')

// Templates
const messageTemplate = document.querySelector('#message-template').innerHTML
const locationMessageTemplate = document.querySelector('#location-message-template').innerHTML
const sidebarTemplate = document.querySelector('#sidebar-template').innerHTML

// Options
const {username, room} = Qs.parse(location.search, { ignoreQueryPrefix: true })

const autoScroll = () => {
    // New message element
    const $newMessage = $messages.lastElementChild

    // Height of the new message
    const newMessageStyles = getComputedStyle($newMessage)
    const newMessageMargin = parseInt(newMessageStyles.marginBottom)
    const newMessageHeight = $newMessage.offsetHeight + newMessageMargin

    const visibleHeight = $messages.offsetHeight


    // height of messsages continaer
    const containerHeight = $messages.scrollHeight

    // How far have i scrolles?
    const scrollOffset = $messages.scrollTop + visibleHeight

    if (containerHeight - newMessageHeight <= scrollOffset) {
        $messages.scrollTop = $messages.scrollHeight
    }

    console.log(newMessageMargin)
}

socket.on('message', (message) => {
    const html = Mustache.render(messageTemplate, {
        username: message.username,
        message: message.text,
        createdAt: moment(message.createdAt).format('h:m a')
    })
    $messages.insertAdjacentHTML('beforeend', html)
    autoScroll()
})

socket.on('locationMessage', (locationMessage) => {
    const html = Mustache.render(locationMessageTemplate, {
        username: locationMessage.username,
        url: locationMessage.url,
        createdAt: moment(locationMessage.createdAt).format('h:m a')
    })
    $messages.insertAdjacentHTML('beforeend', html)
    autoScroll()
})

socket.on('roomData', ({room, users}) => {
    const html = Mustache.render(sidebarTemplate, {
        room,
        users
    })
    document.querySelector('#sidebar').innerHTML = html
})

$messageForm
    .addEventListener('submit', (e) => {
        e.preventDefault()

        $messageFormButton.setAttribute('disabled', 'disabled')

        socket.emit('sendMessage', $input.value, (error) => {

            $messageFormButton.removeAttribute('disabled')
            $input.value = ''
            $input.focus()

            if (error) {
                return console.log(error)
            }
            console.log('the message was delivered')
        })
    })

$sendLocationButton
    .addEventListener('click', () => {
        if (!navigator.geolocation) {
            return alert('Geolocation is not supported by your browser')
        }
        $sendLocationButton.setAttribute('disabled', 'disabled')
        navigator.geolocation.getCurrentPosition((position) => {
            console.log(position)
           socket.emit('sendLocation',
               `https://www.google.com/maps?q=${position.coords.latitude},${position.coords.longitude}`,
               () => {
                $sendLocationButton.removeAttribute('disabled')
                console.log('location shared')
               })
        })
    })

socket.emit('join', {username, room}, (error) => {
    if (error) {
        alert(error)
        location.href = '/'
    }
})
```


## Express-validator

posts.js
```javascript
const {body, validationResult} = require('express-validator')

router.post(
  '/',
  auth,
  [
    body('text', 'text is required').not().isEmpty()
  ],
  async (req, res) => {
    try {

      const errors = validationResult(req)
      if (!errors.isEmpty()) {
        return res.status(400).json({errors: errors.array()})
      }
      const user = await User.findById(req.user.id).select('-password')
      const newPost = new Post({
        text: req.body.text,
        name: user.name,
        avatar: user.avatar,
        user: req.user.id
      })

      const post = await newPost.save()

      return res.json(post)
    } catch (err) {
      console.error(err.message)
      return res.status(500).send('Server Error')
    }
  }
)
```

## Client served from the server

server.js

```javascript
const express = require('content/snippets/nodejs-express')
const connectDB = require('./config/db')

const app = express()

const path = require('path')

connectDB()

app.use(express.json())

// app.get('/', (req, res) => {
//   res.send('API running')
// })

app.use('/api/users', require('./routes/api/users'))
app.use('/api/auth', require('./routes/api/auth'))
app.use('/api/profile', require('./routes/api/profile'))
app.use('/api/posts', require('./routes/api/posts'))

// Serve static assets in production
if (process.env.NODE_ENV === 'production') {
  app.use(express.static('client/build'))

  app.get('*', (req, res) => {
    res.sendFile(path.resolve(__dirname, 'client', 'build', 'index.html'))
  })
}

const PORT = process.env.PORT || 5000

app.listen(PORT, () => {
  console.log(`Server started on port ${PORT}`)
})
```
