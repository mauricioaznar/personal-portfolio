---
title: Mongoose
---

## Models docs

"Models are fancy constructors compiled from Schema definitions
. An instance of a model is called a document. Models are responsible
for creating and reading documents from the underlying MongoDB database."
[mongoose models documentation](https://mongoosejs.com/docs/models.html)

<br />

## Model setups
### Javascript
Explanation (Blog.js & User.js)
1. We declare a model by using the schema property
2. Model gets created once we passed the schema into the model function
Blog.js
```javascript
const mongoose = require('content/snippets/mongoose');
const { Schema } = mongoose;

const blogSchema = new Schema({
  title: String,
  content: String,
  createdAt: { type: Date, default: Date.now },
  _user: { type: Schema.Types.ObjectId, ref: 'User' } // References another model
});

mongoose.model('Blog', blogSchema);
```

User.js

```javascript
const mongoose = require('content/snippets/mongoose');
const { Schema } = mongoose;

const userSchema = new Schema({
  googleId: String,
  displayName: String
});

mongoose.model('User', userSchema);
```
<br></br>
### Typescript
Explanation (Ticket.ts)
* interface TicketAttrs is used to validate parameters in functions (it has match TicketDoc properties)
* interface TicketDoc is used by mongoose to define the document properties and its functions (methods)
* interface TicketModel is used by mongoose to define the model properties and its functions (statics)
* toJson.property on the schema is used to change how the objects gets formatted before sending it into the response
    * example: we can remove passwords or change property names
* `const Ticket = mongoose.model<TicketDoc, TicketModel>('Ticket', ticketSchema);` is used to create the model
* the build method creates a document and uses typescript to aid the programmer as he/she is creating it.


<br></br>
Ticket.ts
```typescript
import mongoose from 'mongoose';
import {updateIfCurrentPlugin} from "mongoose-update-if-current";
import { Order, OrderStatus } from './order';

interface TicketAttrs {
  id: string;
  title: string;
  price: number;
}

export interface TicketDoc extends mongoose.Document {
  title: string;
  price: number;
  version: number;
  isReserved(): Promise<boolean>;
}

interface TicketModel extends mongoose.Model<TicketDoc> {
  build(attrs: TicketAttrs): TicketDoc;
  findByEvent(event: {id: string, version: number}): Promise<TicketDoc | null>;
}

const ticketSchema = new mongoose.Schema(
  {
    title: {
      type: String,
      required: true,
    },
    price: {
      type: Number,
      required: true,
      min: 0,
    },
  },
  {
    toJSON: {
      transform(doc, ret) {
        ret.id = ret._id;
        delete ret._id;
      },
    },
  }
);

ticketSchema.set('versionKey', 'version')
ticketSchema.plugin(updateIfCurrentPlugin)

ticketSchema.statics.findByEvent = (event: {id: string, version: number}) => {
    return Ticket.findOne({
        _id: event.id,
        version: event.version - 1
    })
}
ticketSchema.statics.build = (attrs: TicketAttrs) => {
  return new Ticket({
      _id: attrs.id,
      title: attrs.title,
      price: attrs.price
  });
};

// use function because 'this' to conserve the value of this
ticketSchema.methods.isReserved = async function () {
    // Make sure that this ticket is not already reserved
    // Run query to look at all orders. Find an order where the ticket
    // is the ticket we just found *and* the orders status is *not* cancelled
    // If we find an order from that means the ticket *is* reserved
    const ticket = this as TicketDoc
    const existingOrder = await Order.findOne({
    ticket: ticket,
    status: {
      $in: [
        OrderStatus.Created,
        OrderStatus.AwaitingPayment,
        OrderStatus.Complete,
      ],
    },
  });

  return !!existingOrder;
};

const Ticket = mongoose.model<TicketDoc, TicketModel>('Ticket', ticketSchema);

export { Ticket };
```
<br />
Order.ts

```typescript
import mongoose from 'mongoose';
import { OrderStatus } from '@maguas/common';
import { TicketDoc } from './ticket';
import {updateIfCurrentPlugin} from "mongoose-update-if-current";

export { OrderStatus };

interface OrderAttrs {
  userId: string;
  status: OrderStatus;
  expiresAt: Date;
  ticket: TicketDoc;
}

interface OrderDoc extends mongoose.Document {
  userId: string;
  status: OrderStatus;
  expiresAt: Date;
  version: number;
  ticket: TicketDoc;
}

interface OrderModel extends mongoose.Model<OrderDoc> {
  build(attrs: OrderAttrs): OrderDoc;
}

const orderSchema = new mongoose.Schema(
  {
    userId: {
      type: String,
      required: true,
    },
    status: {
      type: String,
      required: true,
      enum: Object.values(OrderStatus),
      default: OrderStatus.Created,
    },
    expiresAt: {
      type: mongoose.Schema.Types.Date,
    },
    ticket: {
      type: mongoose.Schema.Types.ObjectId,
      ref: 'Ticket',
    },
  },
  {
    toJSON: {
      transform(doc, ret) {
        ret.id = ret._id;
        delete ret._id;
      },
    },
  }
);

orderSchema.set('versionKey', 'version')
orderSchema.plugin(updateIfCurrentPlugin)

orderSchema.statics.build = (attrs: OrderAttrs) => {
  return new Order(attrs);
};

const Order = mongoose.model<OrderDoc, OrderModel>('Order', orderSchema);

export { Order };
```

<br />

## Querying

### findById
```typescript
const ticket = await Ticket.findById(ticketId)
```
<br />

### find
* populates attaches documentes that are related to these. It must have a property with
  type mongoose.Schema.Types.ObjectId (`type: mongoose.Schema.Types.ObjectId`)
```typescript
        const orders = await Order.find({
            userId: req.currentUser!.id
        }).populate('ticket')
        res.send(orders)
```

<br />

### Create (using the previous static build function)
* we call save so that it gets into the database
```typescript
const order = Order.build({
    userId: req.currentUser!.id,
    status: OrderStatus.Created,
    expiresAt: expiration,
    ticket: ticket
})
await order.save()
```

<br />

### Update 
```typescript
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
```

<br/>

## Redis

"Redis is an open source (BSD licensed), in-memory data structure store, 
used as a database, cache, and message broker."
[redis documentation](https://redis.io/documentation)

### Mongoose global object overriding for redis 
*advanced node: stephen grider*
<br></br>
Definitions
*  REDIS.hget Returns the value associated with field in 
   the hash stored at key. ([link](https://redis.io/commands/hget))
<br></br>
Explanation (cache.js)
1. `const client = redis.createClient(keys.redisUrl)` - we connect to 
   redis client through a key stored in an .env file
2. We prepare REDIS.client.hget function and convert it to the async version of it
    1. `const {promisify} = require('util');`
    2. `client.hget = promisify(client.hget)` - we convert to an async await 
3. `const exec = mongoose.Query.prototype.exec` We store the exec so that we can override it and add redis functionality to it.
4. `mongoose.Query.prototype.cache = function (options = { -code- }) {}` We 
   create a function inside the query object to specify we would like to use a hash
   with an optional key
5. we substitute the exec function with the new redis exec function
    1. if previous cache function wasnt invoked
          1. we call exec normally and return
    2. ``const key = JSON.stringify(Object.assign( ... )`` - we create a key for the specific query we are caching
    3. `const cacheValue = await client.hget(this.hashKey, key)` We get the redis client stored value
    4. if there is a cached value
          1. return it as a document array or document
    5. `const result = await exec.apply(this, arguments)` we call exec and store the value
    6. we store the value on the redis client
6. `require('./services/cache')` - require file at index.js (so it gets executed)

* `  clearHash(hashKey) { client.del(JSON.stringify(hashKey)) }` this function is exported so that we can remove
  cached values
<br></br>
cache.js

```javascript
const mongoose = require('content/snippets/mongoose')
const redis = require('redis')
const util = require('util')
const keys = require('../config/keys')

const client = redis.createClient(keys.redisUrl)

const { promisify } = require('util')
client.hget = promisify(client.hget)

const exec = mongoose.Query.prototype.exec

mongoose.Query.prototype.cache = function(options = {}) {
  this.useCache = true
  this.hashKey = JSON.stringify(options.key || '')

  return this
}

mongoose.Query.prototype.exec = async function() {
  if (!this.useCache) {
    return exec.apply(this, arguments)
  }

  const key = JSON.stringify(Object.assign({}, this.getQuery(), {
    collection: this.mongooseCollection.name
  }))

  // See if we have a value for 'key' in redis
  const cacheValue = await client.hget(this.hashKey, key)

  // If we do, return that
  if (cacheValue) {
    // const doc = new this.model(JSON.parse(cacheValue))
    const doc = JSON.parse(cacheValue)
    return Array.isArray(doc)
      ? doc.map(d => new this.model(d))
      : new this.model(doc)
  }

  // Otherwise, issue the query and store the result in redis

  const result = await exec.apply(this, arguments)

  client.hmset(this.hashKey, key, JSON.stringify(result), 'EX', 10)

  return result
}

module.exports = {
  clearHash(hashKey) {
    client.del(JSON.stringify(hashKey))
  }
}
```
<br></br>
Explanation (route.js)
1. We can call any query and cache it by calling the cache at the end of the query.
route.js (cache query)
```javascript
  app.get('/api/blogs', requireLogin, async (req, res) => {
    const blogs = await Blog
      .find({ _user: req.user.id })
      .cache({ key: req.user.id });

    return res.send(blogs);
  });
```
<br></br>
Explanation (route.js)
1. We call cleanCache whenever a new entity gets created by using the cleanCache as a middleware.
route.js
```javascript
  app.post(
    '/api/blogs',
    requireLogin, // middleware for verifying auth user
    cleanCache, 
    async (req, res) => {
      const { title, content } = req.body;
  
      const blog = new Blog({
        title,
        content,
        _user: req.user.id
      });
  
      try {
        await blog.save();
        res.send(blog);
      } catch (err) {
        res.send(400, err);
      }
    });
```

<br />

cleanCache.js
```javascript
const {clearHash} = require('../services/cache')

module.exports = async (req, res, next) => {
  // make sure the route handle does whaterver needs to to
  // and return to this function
  await next()

  clearHash(req.user.id)
}
```


## Useful techniques

### timestamps
Add timestamps to a model

Task.js
```javascript
const mongoose = require('mongoose')
const validator = require('validator')

const taskSchema = mongoose.Schema({
  description: {
    type: String,
    required: true,
    trim: true
  },
  completed: {
    type: Boolean,
    default: false
  },
  owner: {
    type: mongoose.Schema.Types.ObjectId,
    required: true,
    ref: 'User'
  }
}, {
  timestamps: true
})

const Task = mongoose.model('Task', taskSchema)

module.exports = Task
```

### Document toJSON overriding
```javascript
// toJSON is a method used in the stringify process.
// properties are deleted so they remain hidden to the client
userSchema.methods.toJSON = function () {
  const user = this
  const userObject = user.toObject()

  delete userObject.password
  delete userObject.tokens
  delete userObject.avatar

  return userObject
}
```

### Document generate token method
```javascript
// method used on the instance
// normal function was used so this points to the this of the instance (user)
userSchema.methods.generateAuthToken = async function () {
  const user = this
  // token generated encoding the id with the jwt secret
  const token = jwt.sign({_id: user._id.toString() }, process.env.JWT_SECRET)

  user.tokens = user.tokens.concat({ token })
  await user.save()

  return token
}
```

### Model findByCredentials

```javascript
// method used on the class
// arrow function used because this is not important
userSchema.statics.findByCredentials = async (email, password) => {
  const user = await User.findOne({email} )

  if (!user) {
    throw new Error('Unable to login!')
  }

  const isMatch = await bcrypt.compare(password, user.password)

  if (!isMatch) {
    throw new Error('Unable to login!')
  }

  return user
}
```

### Hash before saving

```javascript
// Hash the plain text password before saving
userSchema.pre('save', async function (next) {
  const user = this

  if (user.isModified('password')) {
    user.password = await bcrypt.hash(user.password, 8)
  }

  next()
})

```

### Cascading remove to related entities

```javascript
// cascading the remove method to related tasks
userSchema.pre('remove', async function (next) {
  const user = this

  const result = await Task.deleteMany({owner: user._id})

  next()
})
```

### Virtual property (alternate to populate())

```javascript
// a virtual property is a property that is generated on execution
userSchema.virtual('tasks', {
  ref: 'Task',
  localField: '_id',
  foreignField: 'owner'
})
```


This options need to be defined `{ toObject: { virtuals: true }, toJSON: { virtuals: true } }`
inside of `new mongoose.Schema({ ... }, options: { } }` 


### Advance mongoose schema definition

User.js
```javascript
const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
    trim: true
  },
  age: {
    type: Number,
    default: 0,
    validate(value) {
      if (value < 0) {
        throw new Error('Age must be a positive number')
      }
    }
  },
  password: {
    type: String,
    required: true,
    trim: true,
    validate(value) {
      if (value.length < 6) {
        throw new Error('Password length must be greater than 6')
      }
      if (value.toLowerCase().includes('password')) {
        throw new Error('Password must not contain password')
      }
    }
  },
  email: {
    type: String,
    unique: true,
    required: true,
    trim: true,
    lowercase: true,
    validate(value) {
      if (!validator.isEmail(value)) {
        throw new Error('Email is invalid')
      }
    }
  },
  tokens: [{
    token: {
      type: String,
      required: true
    }
  }],
  avatar: {
    type: Buffer
  }
}, {
  timestamps: true,
  toObject: {
    virtuals: true
  },
  toJSON: {
    virtuals: true
  }
})
```
