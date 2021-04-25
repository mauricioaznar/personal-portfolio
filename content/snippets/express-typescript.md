---
title: Express Typescript
---


## Introduction
* `"@types/mongoose": "^5.10.3",` was used instead of `5.10.4` because of a type bug.
* `ts-node-dev` allos for file reload on change
* `@maguas/common` custom-made package for code reusability
```json
{
  "dependencies": {
    "@maguas/common": "^1.0.15",
    "@types/cookie-session": "^2.0.42",
    "@types/express": "^4.17.11",
    "@types/jsonwebtoken": "^8.5.0",
    "@types/mongoose": "^5.10.3",
    "cookie-session": "^1.4.0",
    "express": "^4.17.1",
    "express-async-errors": "^3.1.1",
    "express-validator": "^6.9.2",
    "jsonwebtoken": "^8.5.1",
    "mongoose": "^5.10.19",
    "mongoose-update-if-current": "^1.4.0",
    "ts-node-dev": "^1.1.1",
    "typescript": "^4.1.5"
  },
  "devDependencies": {
    "@types/jest": "^26.0.20",
    "@types/supertest": "^2.0.10",
    "jest": "^26.6.3",
    "mongodb-memory-server": "^6.9.3",
    "supertest": "^6.1.3",
    "ts-jest": "^26.5.1"
  }
}
```

## Basic setup
Explanation (index.ts)

1. Create an async function `start` and call it at the end of the file
2. Check for keys that are going to get used on the ap
3. Call `mongoose.connect` with those parameters to disable warnings
4. Listen on a port
<br></br>
index.ts
```javascript
import mongoose from 'mongoose'
import {app} from "./app";

const start = async () => {
    console.log('Starting up')
    if (!process.env.JWT_KEY) {
        throw new Error('JWT_KEY must be defined')
    }

    if (!process.env.MONGO_URI) {
        throw new Error('MONGO_URI must be defined')
    }

    try {
        await mongoose.connect(process.env.MONGO_URI, {
            useNewUrlParser: true,
            useUnifiedTopology: true,
            useCreateIndex: true
        })
    } catch (e) {
        console.error(e)
    }

    app.listen(3000, () => {
        console.log('Listening on port 3000')
    })
}

start()
```
Explanation (app.ts)

1. `import 'express-async-errors'` import this file so that 
   async functions can be used in route handlers
2. Import custom error handler and not custom errors `import {errorHandler, NotFoundError} from "@maguas/common";` 
   so that we can send consistent errors
3. `app.use(currentUserRouter)` middleware used for handling user authorization token
<br></br>
app.js
```typescript
import express from 'express'
import 'express-async-errors'
import {json} from 'body-parser'
import {errorHandler, NotFoundError} from "@maguas/common";
import cookieSession from "cookie-session";

import {currentUserRouter} from "./routes/current-user";
import {signInRouter} from "./routes/signin";
import {signOutRouter} from "./routes/signout";
import {signUpRouter} from "./routes/signup";

const app = express()
// trust ingress nginx
app.set('trust proxy', true)
app.use(json())
// disable encryption and enable https
app.use(
    cookieSession({
        signed: false,
        secure: process.env.NODE_ENV !== 'test'
    })
)

app.use(currentUserRouter)
app.use(signInRouter)
app.use(signOutRouter)
app.use(signUpRouter)

app.all('*', async (req, res) => {
    console.log('star')
    throw new NotFoundError()
})

// error handler is used at the end
// so that we can catch any error thrown previously and format it
app.use(errorHandler)

export { app }
```

<br />

## Security

scripts used for hashing, password, validations

### Password

```typescript
import {scrypt, randomBytes} from 'crypto'
import {promisify} from 'util'

// convert to async await
const scryptAsync = promisify(scrypt)

export class Password {

    static async toHash(password: string) {
        const salt = randomBytes(8).toString('hex')
        const buf = (await scryptAsync(password, salt, 64)) as Buffer
        return `${buf.toString('hex')}.${salt}`
    }

    static async compare(storedPassword: string, suppliedPassword: string) {
        const [hashedPassword, salt] = storedPassword.split('.')
        const buf = (await scryptAsync(suppliedPassword, salt, 64)) as Buffer
        return buf.toString('hex') === hashedPassword
    }
}

```

<br />

## Errors

List of errors that are returned to the client as an array of strings.
<br></br>

### Custom error class
```typescript
export abstract class CustomError extends Error {

    abstract statusCode: number


    constructor(message: string) {
        super(message);

        Object.setPrototypeOf(this, CustomError.prototype)
    }

    abstract serializeErrors(): {
        message: string;
        field?: string;
    }[]
}
```

<br />

### Bad request 400
*implements custom error*
```typescript
import {CustomError} from "./custom-error";

export class BadRequestError extends CustomError {

    constructor(public message: string) {
        super(message);

        Object.setPrototypeOf(this, BadRequestError.prototype)
    }

    statusCode = 400;

    serializeErrors() {
        return [
            {
                message: this.message
            }
        ];
    }

}
```

<br />

### Database error 500
*implements custom error*
```typescript
import {CustomError} from "./custom-error";

export class DatabaseConnectionError extends CustomError {
    reason = 'Error connecting to database'
    statusCode = 500

    constructor() {
        super('Database connection error');

        Object.setPrototypeOf(this, DatabaseConnectionError.prototype)
    }

    serializeErrors() {
        return [
            {
                message: this.reason
            }
        ]
    }
}
```

<br />

### Not authorized 401
*implements custom error*
```typescript
import {CustomError} from "./custom-error";

export class NotAuthorizedError extends CustomError {
statusCode = 401;

    constructor() {
        super('Not authorized')

        Object.setPrototypeOf(this, NotAuthorizedError.prototype)
    }

    serializeErrors() {
        return [
            {
                message: 'Not authorized'
            }
        ];
    }

}
```

<br />

### Not found 404
```typescript
import {CustomError} from "./custom-error";

export class NotFoundError extends CustomError {
    statusCode = 404;

    constructor() {
        super('Route not found');

        Object.setPrototypeOf(this, NotFoundError.prototype)
    }

    serializeErrors(): { message: string; field?: string }[] {
        return [
            {
                message: 'Not found'
            }
        ];
    }
    // statusCode
}
```

<br />

### Request validation error 400
*implements custom error*
*uses express validator*
```typescript
import {ValidationError} from 'express-validator'
import {CustomError} from "./custom-error";

export class RequestValidationError extends CustomError {

    statusCode = 400

    constructor(public errors: ValidationError[]) {
        super('Validation error')

        // Only because we are extending a built in class
        Object.setPrototypeOf(this, RequestValidationError.prototype)
    }

    serializeErrors() {
        return this.errors.map(err => {
            return {message: err.msg, field: err.param}
        })
    }
}

```

<br />

## Middlewares

### Error handler
Returns an array of strings of errors regardless of the error type 
```typescript
import { Request, Response, NextFunction } from 'express'
import {CustomError} from "../errors/custom-error";

export const errorHandler = (
    err: Error,
    req: Request,
    res: Response,
    next: NextFunction
) => {

    if (err instanceof CustomError) {
        return res.status(err.statusCode).send({errors: err.serializeErrors()})
    }

    console.error(err)

    res.status(400).send({
        errors: [
            {
                message: 'Something went wrong'
            }
        ]
    })
}
```

### Verify user
*rejects if there is not a user defined in the request*
```typescript
import {Response, Request, NextFunction} from "express";
import {NotAuthorizedError} from "../errors/not-authorized-error";

export const requireAuth = (req: Request, res: Response, next: NextFunction) => {
    if (!req.currentUser) {
        throw new NotAuthorizedError()
    }
    next()
}
```

### Current user (merges current user to the request object)
*uses jsonwebtoken*
```typescript
import {Request, Response, NextFunction} from "express";
import jwt from 'jsonwebtoken'

interface UserPayload {
    id: string;
    email: string;
}

declare global {
    namespace Express {
        interface Request {
            currentUser?: UserPayload
        }
    }
}

export const currentUser = (req: Request, res: Response, next: NextFunction) => {
    if (!req.session?.jwt) {
        return next()
    }

    try {
        const payload = jwt.verify(req.session.jwt, process.env.JWT_KEY!) as UserPayload
        req.currentUser = payload
    } catch(e) {
        console.error(e)
    }
    next()
}
```

<br />


### Validation request (express-validator)

```typescript
import {Request, Response, NextFunction} from "express";
import {validationResult} from "express-validator";
import {RequestValidationError} from "../errors/request-validation-error";

export const validateRequest = (req: Request, res: Response, next: NextFunction) => {
    const errors = validationResult(req)

    if (!errors.isEmpty()) {
        throw new RequestValidationError(errors.array())
    }

    next()
}
```

<br />


## Models
* We supply the application with entities that have some properties and functions helping storing
some data into the mongo database
  
### User

* Mongoose has a special function called `pre` that runs prior to some event. 
    * In this case, we ran it before it gets saved into the database, so that we can change its password value
      for a hashed value.
* We also deleted the password before being sent to the client in the response.
```typescript
import mongoose from 'mongoose'
import {Password} from '../services/password'

// An interface that describes the properties
// that are required to create a new User

interface UserAttrs {
    email: string;
    password: string;
}

// An interface that describes the properties
// that a User model has

interface UserModel extends mongoose.Model<UserDoc> {
    build(attrs: UserAttrs): UserDoc
}

// An interface that describes the properties
// that a User Document has

interface UserDoc extends mongoose.Document {
    email: string;
    password: string;
}

const userSchema = new mongoose.Schema({
    email: {
        type: String,
        required: true
    },
    password: {
        type: String,
        required: true
    }
}, {
    toJSON: {
        transform: (doc, ret, options) => {
            ret.id = ret._id
            delete ret._id
            delete ret.password
            delete ret.__v
        }
    }
})

userSchema.pre('save', async function (done) {
    if (this.isModified('password')) {
        const hashed = await Password.toHash(this.get('password'))
        this.set('password', hashed)
    }
    done()
})

userSchema.statics.build = (attrs: UserAttrs) => {
    return new User(attrs)
}

const User = mongoose.model<UserDoc, UserModel>('User', userSchema)

export {User}
```


<br />

## Routing

*Here it is being shown some examples that put together
the middlewares, errors, and mongoose models. Plus some validation is added*
### Set up
app.js
```typescript
import express from 'express'
import 'express-async-errors'

import {json} from 'body-parser'

const app = express()

app.use(newOrderRouter)
app.use(showOrderRouter)
app.use(indexOrderRouter)
app.use(deleteOrderRouter)
```

### Sign in
* uses cookie-session (see setup)
    * needs to store something on the session property. We decided to store a jwt (json web token).
* validate request goes after the body validations to catch any error and send it on the response


```typescript
import express, {Request, Response} from 'express'
import {body, validationResult} from "express-validator"
import jwt from "jsonwebtoken";

import {BadRequestError} from "@maguas/common";
import {validateRequest} from "@maguas/common";
import {User} from '../models/user'
import {Password} from "../services/password";

const router = express.Router()

router.post(
    '/api/users/signin',
    [
        body('email').isEmail().withMessage('Email must be valid'),
        body('password')
            .trim()
            .isLength({min: 4, max: 20})
            .withMessage('Password must be between 4 and 20 characters')
    ],
    validateRequest,
    async (req: Request, res: Response) => {

        const {email, password} = req.body

        const existingUser = await User.findOne({ email })

        if (!existingUser) {
            throw new BadRequestError('Invalid credentials')
        }

        const passwordsMatch = await Password.compare(existingUser.password, password)

        if (!passwordsMatch) {
            throw new BadRequestError('Invalid credentials')
        }

        // Generate JWT
        const userJwt = jwt.sign({
            id: existingUser.id,
            email: existingUser.email
        }, process.env.JWT_KEY!)

        // store it on session object
        req.session = {
            jwt: userJwt
        }

        res.status(200).send(existingUser)
    }
)

export { router as signInRouter }
```

<br />

### Sign out
*We set the session property to null and gets sent back to the client*
```typescript
import express from 'express'

const router = express.Router()

router.post('/api/users/signout', (req, res) => {
    req.session = null
    res.send({})
})

export { router as signOutRouter }
```

<br />

### Sign up
* Similarly to sign in, we create a jwt and send it back to the client if email and password are valid
```typescript
import express, {Request, Response} from 'express'
import {body} from "express-validator";
import jwt from 'jsonwebtoken'

import {validateRequest, BadRequestError} from "@maguas/common";
import {User} from '../models/user'


const router = express.Router()

router.post(
    '/api/users/signup',
    [
        body('email').isEmail().withMessage('Email must be valid'),
        body('password')
            .trim()
            .isLength({min: 4, max: 20})
            .withMessage('Password must be between 4 and 20 characters')
    ],
    validateRequest,
    async (req: Request, res: Response) => {

        const {email, password} = req.body

        const existingUser = await User.findOne({email})

        if (existingUser) {
            throw new BadRequestError('Email is in use')
        }

        const user = User.build({email, password})

        await user.save()

        // Generate JWT
        const userJwt = jwt.sign({
            id: user.id,
            email: user.email
        }, process.env.JWT_KEY!)

        // store it on session object
        req.session = {
            jwt: userJwt
        }

        res.status(201).send(user)
    }
)

export { router as signUpRouter }

```

<br />

### Post
*This file must be used on the main file so that express knows of its existence*


```typescript
import mongoose from 'mongoose'
import express, { Request, Response } from 'express'
import {BadRequestError, NotFoundError, OrderStatus, requireAuth, validateRequest} from "@maguas/common";
import {body} from 'express-validator'
import {Ticket} from "../models/ticket";
import {Order} from "../models/order";


const router = express.Router()

const EXPIRATION_WINDOW_SECONDS = 1 * 60

router.post(
    '/api/orders',
    requireAuth,
    [
        body('ticketId')
            .not()
            .isEmpty()
            .custom((input: string) => {
                return mongoose.Types.ObjectId.isValid(input)
            })
            .withMessage('ticketId must be provided')
    ],
    validateRequest,
    async (req: Request, res: Response) => {
        const {ticketId} = req.body

        // Find the ticket the user is trying to order in the database
        const ticket = await Ticket.findById(ticketId)
        if (!ticket) {
            throw new NotFoundError()
        }

        const isReserved = await ticket.isReserved()
        if (isReserved) {
            throw new BadRequestError('Ticket is already reserved')
        }

        // Calculate an expiration date for this order
        const expiration = new Date()
        expiration.setSeconds(expiration.getSeconds() + EXPIRATION_WINDOW_SECONDS)

        // Build the order and save it to the database
        const order = Order.build({
            userId: req.currentUser!.id,
            status: OrderStatus.Created,
            expiresAt: expiration,
            ticket: ticket
        })
        await order.save()
      
        res.status(201).send(order)
    }
)

export { router as newOrderRouter }
```

<br />

### Get many
* The populate methods is used to attach documents that are related to these (see mongo)
```typescript
import express, { Request, Response } from 'express'
import {requireAuth} from "@maguas/common";
import {Order} from "../models/order";

const router = express.Router()

router.get(
    '/api/orders',
    requireAuth,
    async (req: Request, res: Response) => {
        const orders = await Order.find({
            userId: req.currentUser!.id
        }).populate('ticket')
        res.send(orders)
    }
)

export { router as indexOrderRouter }
```

### Get one 

```typescript
import express, { Request, Response } from 'express'
import {NotAuthorizedError, NotFoundError, requireAuth} from "@maguas/common";
import {Order} from "../models/order";

const router = express.Router()

router.get(
    '/api/orders/:orderId',
    requireAuth,
    async (req: Request, res: Response) => {
        const order = await Order
            .findById(req.params.orderId)
            .populate('ticket')
        if (!order) {
            throw new NotFoundError()
        }
        if (order.userId !== req.currentUser!.id) {
            throw new NotAuthorizedError()
        }
        res.send(order)
    }
)

export { router as showOrderRouter }
```

### Update

```typescript
import express, {Request, Response} from 'express'
import {body} from 'express-validator'
import {BadRequestError, NotAuthorizedError, NotFoundError, requireAuth, validateRequest} from "@maguas/common";
import {Ticket} from "../models/ticket";

const router = express.Router()

router.put('/api/tickets/:id',
    requireAuth,
    [
        body('title')
            .not()
            .isEmpty()
            .withMessage('Title is required'),
        body('price')
            .isFloat({ gt: 0 })
            .withMessage('Price must be provided and greater than 0')
    ],
    validateRequest,
    async (req: Request, res: Response) => {
        const ticket = await Ticket.findById(req.params.id)

        if (!ticket) {
            throw new NotFoundError()
        }

        if (ticket.orderId) {
            throw new BadRequestError('Cannot edit a reserved ticket')
        }

        if (ticket.userId !== req.currentUser!.id) {
            throw new NotAuthorizedError()
        }



        ticket.set({
            title: req.body.title,
            price: req.body.price
        })

        await ticket.save()
      
        res.send(ticket)
    }
)

export {router as updateTicketRouter}
```

<br />


## Testing


### Setup

* This file gets run by jest before test execution
* mongodb-memory-server is a library that let us create a mongo db inside memory
```typescript
import {MongoMemoryServer} from 'mongodb-memory-server'
import mongoose from 'mongoose'
import {app} from '../app'
import request from "supertest";
import jwt from 'jsonwebtoken'

declare global {
    namespace NodeJS {
        interface Global {
            signin () : string[]
        }
    }
}

let mongo: any;
beforeAll(async () => {
    process.env.JWT_KEY = 'asdf'

    mongo = new MongoMemoryServer()
    const mongoUri = await mongo.getUri()

    await mongoose.connect(mongoUri, {
        useNewUrlParser: true,
        useUnifiedTopology: true
    })
})

// We delete each collection before each test
beforeEach(async () => {
    jest.clearAllMocks()
    const collections = await mongoose.connection.db.collections()

    for (let collection of collections) {
        await collection.deleteMany({})
    }
})

// We clear mongo db memory server to avoid leaks
afterAll(async () => {
    await mongo.stop()
    await mongoose.connection.close()
})

// we create a function inside the global object that let us retrieve a valid cookie session
global.signin = () => {
    // build a JWT payload. {id, email}
    const payload = {
        id: new mongoose.Types.ObjectId().toHexString(),
        email: 'test@test.com'
    }

    // create the jwt!
    const token = jwt.sign(payload, process.env.JWT_KEY!)

    // build session Object. { jwt: MY_JWT }
    const session = {jwt: token}

    // turn that session into JSON
    const sessionJSON = JSON.stringify(session)

    // take json and encode it as base64
    const base64 = Buffer.from(sessionJSON).toString('base64')

    // return a string thats the cookie with the encoded data
    return [`express:sess=${base64}`]
}
```

<br />


### Get many
* We create a function to create an entity an reuse it in the test.
* The global signin function gets use to create a valid session cookie
```typescript
import request from 'supertest'
import {app} from "../../app";

const createTicket = () => {
    return request(app)
        .post('/api/tickets')
        .set('Cookie', global.signin())
        .send({
            title: 'aslfkf',
            price: 20
        })
}

it ('can fetch a list of tickets', async () => {
    await createTicket()
    await createTicket()
    await createTicket()

    const response = await request(app)
        .get('/api/tickets')
        .send()
        .expect(200)

    expect(response.body.length).toEqual(3)
})
```

<br />

### Post
```typescript
import request from 'supertest'
import {app} from '../../app'
import {Ticket} from '../../models/ticket'

it('has a route handler listening to /api/tickets for post requests', async () => {
    const response = await request(app)
        .post('/api/tickets')
        .send({})

    expect(response.status).not.toEqual(404)
})

it('can only be access if the user is signed in', async () => {
    const response = await request(app)
        .post('/api/tickets')
        .send({})
        .expect(401)

})

it('returns a status other than 401 if the user is signed in', async () => {
    const response = await request(app)
        .post('/api/tickets')
        .set('Cookie', global.signin())
        .send({})

    expect(response.status).not.toEqual(401)

})


it('returns an error if an invalid title is provided', async () => {
    await request(app)
        .post('/api/tickets')
        .set('Cookie', global.signin())
        .send({
            title: '',
            price: 10
        })
        .expect(400)

    await request(app)
        .post('/api/tickets')
        .set('Cookie', global.signin())
        .send({
            price: 10
        })
        .expect(400)
})

it('returns an error if an invalid prices is provided', async () => {
    await request(app)
        .post('/api/tickets')
        .set('Cookie', global.signin())
        .send({
            title: 'asdfasdfasd',
            price: -10
        })
        .expect(400)

    await request(app)
        .post('/api/tickets')
        .set('Cookie', global.signin())
        .send({
            price: -10
        })
        .expect(400)
})

it('creates a ticket with valid inputs', async () => {
    let tickets = await Ticket.find({})
    expect(tickets.length).toEqual(0)

    await request(app)
        .post('/api/tickets')
        .set('Cookie', global.signin())
        .send({
            title: 'asldkfj',
            price: 20
        })

    tickets = await Ticket.find({})
    expect(tickets.length).toEqual(1)
    expect(tickets[0].title).toEqual('asldkfj')
})
```

<br />

### Gets one

```typescript
import request from 'supertest'
import {app} from "../../app";
import mongoose from "mongoose";

it('returns a 404 if the ticket is not found', async () => {
    const id = new mongoose.Types.ObjectId().toHexString()
    await request(app)
        .get(`/api/tickets/${id}`)
        .send()
        .expect(404)
})

it('returns the ticket if the ticket is found', async () => {
    const title = 'Concert'
    const price = 20

    const response = await request(app)
        .post('/api/tickets')
        .set('Cookie', global.signin())
        .send({
            title,
            price
        })

    const ticketResponse = await request(app)
        .get(`/api/tickets/${response.body.id}`)
        .send()
        .expect(200)

    expect(ticketResponse.body.title).toEqual(title)
    expect(ticketResponse.body.price).toEqual(price)
})

```

<br />

### Update

```typescript
import request from "supertest";
import {app} from "../../app";
import mongoose from 'mongoose'
import {Ticket} from "../../models/ticket";


it ('returns a 404 if the provided id does not exist', async () => {
  const id = new mongoose.Types.ObjectId().toHexString()

  await request(app)
    .put(`/api/tickets/${id}`)
    .set('Cookie', global.signin())
    .send({
      title: 'aslsdf',
      price: 20
    })
    .expect(404)
})

it ('returns a 401 if the user is not authenticated', async () => {
  const id = new mongoose.Types.ObjectId().toHexString()

  await request(app)
    .put(`/api/tickets/${id}`)
    .send({
      title: 'aslsdf',
      price: 20
    })
    .expect(401)
})

it ('returns a 401 if the user does not own the ticket', async () => {
  const response = await request(app)
    .post('/api/tickets')
    .set('Cookie', global.signin())
    .send({
      title: 'asldkfj',
      price: 20
    })

  await request(app)
    .put(`/api/tickets/${response.body.id}`)
    .set('Cookie', global.signin())
    .send({
      title: 'asldkfjasdfa',
      price: 30
    })
    .expect(401)
})

it ('returns a 400 if the user provides an invalid title or price', async () => {
  const cookie = global.signin()

  const response = await request(app)
    .post('/api/tickets')
    .set('Cookie', cookie)
    .send({
      title: 'asldkfj',
      price: 20
    })

  await request(app)
    .put(`/api/tickets/${response.body.id}`)
    .set('Cookie', cookie)
    .send({
      title: 'asdfa',
      price: -123
    })
    .expect(400)


})

it ('updates the ticket provided valid inputs', async () => {
  const cookie = global.signin()

  const response = await request(app)
    .post('/api/tickets')
    .set('Cookie', cookie)
    .send({
      title: 'asldkfj',
      price: 20
    })

  await request(app)
    .put(`/api/tickets/${response.body.id}`)
    .set('Cookie', cookie)
    .send({
      title: 'asdfa',
      price: 100
    })
    .expect(200)

  const ticketResponse = await request(app)
    .get(`/api/tickets/${response.body.id}`)
    .send()

  expect(ticketResponse.body.title).toEqual('asdfa')
  expect(ticketResponse.body.price).toEqual(100)
})

it('rejects updates if the ticket is reserved', async () => {
  const cookie = global.signin()

  const response = await request(app)
    .post('/api/tickets')
    .set('Cookie', cookie)
    .send({
      title: 'asldkfj',
      price: 20
    })

  const ticket = await Ticket.findById(response.body.id)
  ticket!.set({
    orderId: mongoose.Types.ObjectId().toHexString()
  })
  await ticket!.save()

  await request(app)
    .put(`/api/tickets/${response.body.id}`)
    .set('Cookie', cookie)
    .send({
      title: 'asdfa',
      price: 100
    })
    .expect(400)
})
```

<br />

### Current user

```typescript
import request from 'supertest'
import {app} from "../../app";

it('responds with details about the current user', async () => {
    const cookie = await global.signin()

    const response = await request(app)
        .get('/api/users/currentuser')
        .set('Cookie', cookie)
        .send({})
        .expect(200)


    expect(response.body.currentUser.email).toEqual('test@test.com')

})

it('responds with null if not authenticated', async () => {
    const response = await request(app)
        .get('/api/users/currentuser')
        .send()
        .expect(200)

    expect(response.body.currentUser).toEqual(null)
})
```

<br />

### Sign up

```typescript
import request from 'supertest'
import {app} from "../../app";

it('returns a 201 on successful signup', async () => {
    return request(app)
        .post('/api/users/signup')
        .send({
            email: 'test@test.com',
            password: 'password'
        })
        .expect(201)
})

it ('returns a 400 with an invalid password', async () => {
    return request(app)
        .post('/api/users/signup')
        .send({
            email: 'test@test.com',
            password: 'p'
        })
        .expect(400)
})

it ('returns a 400 with an invalid email', async () => {
    return request(app)
        .post('/api/users/signup')
        .send({
            email: 'testtest.com',
            password: 'password'
        })
        .expect(400)
})

it ('returns a 400 with missing email and password', async () => {
    await request(app)
        .post('/api/users/signup')
        .send({
            email: 'test@test.com'
        })
        .expect(400)

    await request(app)
        .post('/api/users/signup')
        .send({
            password: 'asdfasdfasdf'
        })
        .expect(400)
})

it('disallows duplicate emails', async () => {
    await request(app)
        .post('/api/users/signup')
        .send({
            email: 'test@test.com',
            password: 'password'
        })
        .expect(201)

    await request(app)
        .post('/api/users/signup')
        .send({
            email: 'test@test.com',
            password: 'password'
        })
        .expect(400)
})

it('sets a cookie after successful signup', async () => {
    const response = await request(app)
        .post('/api/users/signup')
        .send({
            email: 'test@test.com',
            password: 'password'
        })
        .expect(201)

    expect(response.get('Set-Cookie')).toBeDefined()
})
```

<br />

### Sign in

```typescript
import request from 'supertest'
import {app} from "../../app";

it('fails when a email that does not exist is supplied', async () => {
    await request(app)
        .post('/api/users/signin')
        .send({
            email: 'test@test.com',
            password: 'password'
        })
        .expect(400)
})

it('fails when an incorrect password is supplied', async () => {
    await request(app)
        .post('/api/users/signup')
        .send({
            email: 'test@test.com',
            password: 'password'
        })
        .expect(201)

    await request(app)
        .post('/api/users/signin')
        .send({
            email: 'test@test.com',
            password: 'password'
        })
        .expect(200)
})

it('responds with a cookie when given valid credentials', async () => {
    await request(app)
        .post('/api/users/signup')
        .send({
            email: 'test@test.com',
            password: 'password'
        })
        .expect(201)

    const response = await request(app)
        .post('/api/users/signin')
        .send({
            email: 'test@test.com',
            password: 'password'
        })
        .expect(200)

    expect(response.get('Set-Cookie')).toBeDefined()
})
```

<br />

### Sign out

```typescript
import request from 'supertest'
import {app} from "../../app";

it('clears the cookie after signing out', async () => {
    await request(app)
        .post('/api/users/signup')
        .send({
            email: 'test@test.com',
            password: 'password'
        })
        .expect(201)

    const response = await request(app)
        .post('/api/users/signout')
        .send({})
        .expect(200)

    expect(response.get('Set-Cookie')[0])
        .toEqual('express:sess=; path=/; expires=Thu, 01 Jan 1970 00:00:00 GMT; httponly')

})

```
