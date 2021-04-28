---
title: Vue 3 (Real time chat app)
---

## Introduction 

Vue (pronounced /vjuː/, like view) is a progressive framework for building user interfaces.
Unlike other monolithic frameworks, Vue is designed from the ground up to be
incrementally adoptable. The core library is focused on the view layer only,
and is easy to pick up and integrate with other libraries or existing projects. 
On the other hand, Vue is also perfectly capable of powering sophisticated
Single-Page Applications when used in combination with modern tooling and
supporting libraries ([vue 3 docs](https://v3.vuejs.org/guide/introduction.html))


## Main files

package.json
```json
{
  "name": "live-chat",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build"
  },
  "dependencies": {
    "core-js": "^3.6.5",
    "date-fns": "^2.16.1",
    "firebase": "^8.1.2",
    "vue": "^3.0.0",
    "vue-router": "^4.0.0-0"
  },
  "devDependencies": {
    "@vue/cli-plugin-babel": "~4.5.0",
    "@vue/cli-plugin-router": "~4.5.0",
    "@vue/cli-service": "~4.5.0",
    "@vue/compiler-sfc": "^3.0.0"
  }
}
```

App.js
```javascript
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'

import './assets/main.css'

import {projectAuth} from './firebase/config'

let app

projectAuth.onAuthStateChanged(() => {
	if (!app) {
		app = createApp(App)
			.use(router)
			.mount('#app')
	}
})
```

<br />

App.vue
```vue
<template>
  <router-view/>
</template>
```

<br />

## Main components (views)

All components use the set up function

<br />

### Example #1 (refs, functions)


Welcome.vue
```vue
<template>
	<div class="welcome container">
		<p>Welcome</p>
		<div v-if="formToggle">
			<LoginForm @login="enterChat"/>
			<p>No account yet? <span @click="updateToggle">Sign up</span> instead</p>
		</div>
		<div v-else>
			<SignupForm @signup="enterChat" />
			<p>Already registerd? <span @click="updateToggle">Log in</span> instead</p>
		</div>
	</div>
</template>

<script>
	import SignupForm
		from '../components/SignupForm'
	import LoginForm
		from '../components/LoginForm'
	import {ref} from 'vue'
	import {useRouter} from 'vue-router'
	export default {
		components: {
			SignupForm,
			LoginForm
		},
		setup () {
			let formToggle = ref(false)

			const router = useRouter()

			const updateToggle = () => {
				formToggle.value = !formToggle.value
			}

			const enterChat = () => {
				router.push({name: 'Chatroom'})
			}

			return {formToggle, updateToggle, enterChat}
		}
	}
</script>
```
<br />

## Example 2 (watch, composables)

Chatroom.vue
```vue
<template>
	<div class="container">
		<Navbar />
		<ChatWindow />
		<NewChatForm />
	</div>
</template>

<script>
	import Navbar from '../components/Navbar'
	import {watch} from 'vue'
	import getUser from '../composables/getUser'
	import {useRouter} from 'vue-router'
	import NewChatForm from '../components/NewChatForm'
	import ChatWindow from '../components/ChatWindow'
	export default {
		name: 'Chatroom',
		setup() {
			const {user} = getUser()
			const router = useRouter()
			watch(user, () => {
				if (!user.value) {
					router.push({name: 'Welcome'})
				}
			})
		},
		components: {
			Navbar,
			NewChatForm,
			ChatWindow
		}
	}
</script>
```

<br />

## Router 

### Example #1 (router with middlewares)


./router/index.js
```javascript
import { createRouter, createWebHistory } from 'vue-router'
import Welcome from '../views/Welcome'
import Chatroom from '../views/Chatroom'
import {projectAuth} from '../firebase/config'

const requireAuth = (to, from, next) => {
  let user = projectAuth.currentUser
  console.log('current user in the auth guard:', user)
  if (user) {
    next()
  } else {
    next({name: 'Welcome'})
  }
  next()
}

const requireNoAuth = (to, from, next) => {
  let user = projectAuth.currentUser
  if (user) {
    next({name: 'Chatroom'})
  } else {
    next()
  }
}

const routes = [
  {
    path: '/',
    name: 'Welcome',
    component: Welcome,
    beforeEnter: requireNoAuth
  },
  {
    path: '/chatroom',
    name: 'Chatroom',
    component: Chatroom,
    beforeEnter: requireAuth
  }
]

const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes
})

export default router
```

<br />

## Composables

### Example #1 

ref acts like state

useSignup.js
```javascript
import {ref} from 'vue'
import {projectAuth} from '../firebase/config'

const error = ref(null)

const signup = async (email, password, displayName) => {
	error.value = null

	try {
		const res = await projectAuth.createUserWithEmailAndPassword(email, password)
		if (!res) {
			throw new Error('Could not complete the signup')
		}
		await res.user.updateProfile({displayName})
		error.value = null
		return res
	} catch (e) {
		console.log(e.message)
		error.value = e.message
	}
}

const useSignup = () => {

	return {error, signup}
}

export default useSignup
```

<br />

### Example #2

useLogout.js
```javascript
import {ref} from 'vue'
import {projectAuth} from '../firebase/config'

const error = ref(null)

const logout = async () => {
	error.value = null

	try {
		await projectAuth.signOut()
	} catch (e) {
		console.log(e.message)
		error.value = e.message
	}
}

const useLogout = () => {
	return { logout, error }
}

export default useLogout
```

<br />

### Example #3

useLogin.js
```javascript
import {ref} from 'vue'
import {projectAuth} from '../firebase/config'

const error = ref(null)

const login = async (email, password) => {
	error.value = null

	try {
		const res = await projectAuth.signInWithEmailAndPassword(email, password)
		error.value = null
		console.log(res)
		return res
	} catch (e) {
		console.log(e.value)
		error.value = 'Incorrect login credentials'
	}
}

const useLogin = () => {
	return {error, login}
}

export default useLogin
```

<br />

### Example #4

useCollection.js
```javascript
import {ref} from 'vue'
import {projectFirestore} from '../firebase/config'

const useCollection  = (collection) => {
	const error = ref(null)

	const addDoc = async (doc) => {
		error.value = null
		try {
			await projectFirestore.collection(collection).add(doc)
		} catch (e) {
			console.log(e.message)
			error.value = 'could not send the message'
		}
	}

	return {addDoc, error}
}

export default useCollection
```

<br />

### Example #5

```javascript
import {ref} from 'vue'
import {projectAuth} from '../firebase/config'

const user = ref(projectAuth.currentUser)

projectAuth.onAuthStateChanged((_user) => {
	console.log('User state change. Current user is: ', _user)
	user.value = _user
})

const getUser = () => {
	return {user}
}

export default getUser
```

### Example 6

```javascript
import {ref, watchEffect} from 'vue'
import {projectFirestore} from '../firebase/config'

const getCollection = (collection) => {
	const documents = ref(null)
	const error = ref(null)

	let collectionRef = projectFirestore.collection(collection)
		.orderBy('createdAt')

	const unsub = collectionRef.onSnapshot((snap) => {
		let results = []
		snap.docs.forEach(doc => {
			doc.data().createdAt && results.push({...doc.data(), id: doc.id})
		})
		documents.value = results
		error.value = null
	}, (err) => {
		console.log(err.message)
		document.value = null
		error.value = 'Could not fetch data'
	})

	watchEffect((onInvalidate) => {
		onInvalidate(() => {
			console.log('unsubscribed from ', collection)
			unsub()
		})
	})

	return {documents, error}

}

export default getCollection
```

<br />

## Reusable components


### Example 1 

SignupForm.vue
```vue
<template>
	<form @submit.prevent="handleSubmit">
		<input
			type="text"
			required
			placeholder="display name"
			v-model="displayName"
		/>
		<input
			type="email"
			required
			placeholder="email"
			v-model="email"
		/>
		<input
			type="password"
			required
			placeholder="password"
			v-model="password"
		/>
		<div class="error">
			{{ error }}
		</div>
		<button>Sign up</button>
	</form>
</template>

<script>
	import {ref} from 'vue'
	import useSignup
		from '../composables/useSignup'
	export default {
		setup(props, context) {
			const {error, signup} = useSignup()

			const displayName = ref('')
			const email = ref('')
			const password = ref('')

			const handleSubmit = async () => {
				await signup(email.value, password.value, displayName.value)
				if (!error.value) {
					context.emit('signup')
				}
			}

			return { displayName, email, password, handleSubmit, error }
		}
	}
</script>

<style scoped>

</style>
```

<br />

### Example #2 

NewChatForm.vue
```vue
<template>
	<form>
		<textarea
			placeholder="Type a message and hit enter to send"
			v-model="message"
			@keypress.enter.prevent="handleSubmit"
		>
		</textarea>
		<div class="error">
			{{error}}
		</div>
	</form>
</template>

<script>
	import {ref} from 'vue'
	import getUser
		from '../composables/getUser'
	import {timestamp} from '../firebase/config'
	import useCollection
		from '../composables/useCollection'
	export default {
		setup() {
			const {user} = getUser()
			const {addDoc, error} = useCollection('messages')
			const message = ref('')

			const handleSubmit = async () => {
				const chat = {
					message: message.value,
					name: user.value.displayName,
					createdAt: timestamp()
				}
				await addDoc(chat)
				console.log('is submitting')
				if (!error.value) {
					message.value = ''
				}
			}
			return {message, handleSubmit, error}
		}
	}
</script>

<style scoped>
	form {
		margin: 10px;
	}
	textarea {
		width: 100%;
		max-width: 100%;
		margin-bottom: 6px;
		padding: 10px;
		box-sizing: border-box;
		border: 0;
		border-radius: 20px;
		font-family: inherit;
		outline: none;
	}
</style>
```

<br />

### Example #3

```vue
<template>
	<nav v-if="user">
		<div>
			<p>Hey there {{user.displayName}}</p>
			<p class="email">Currently logged in as {{user.email}}</p>
		</div>
		<button @click="handleClick">logout</button>
	</nav>
</template>

<script>
	import useLogout from '../composables/useLogout'
	import getUser from '../composables/getUser'
	export default {
		setup () {
			const {logout, error} = useLogout()
			const {user} = getUser()

			const handleClick = async () => {
				await logout()
				if (!error.value) {
					console.log('user logged o ut')
				}
			}

			return {logout, error, handleClick, user}
		}
	}
</script>

<style scoped>
	nav {
		padding: 20px;
		border-bottom: 1px solid #eee;
		display: flex;
		justify-content: space-between;
		align-items: center;
	}
	nav p {
		margin: 2px auto;
		font-size: 16px;
		color: #444;
	}
	nav p.email {
		font-size: 14px;
		color: #999;
	}
</style>
```

<br />

### Example #4

LoginForm.vue
```vue
<template>
	<form @submit.prevent="handleSubmit">
		<input
			type="email"
			required
			placeholder="email"
			v-model="email"
		/>
		<input
			type="password"
			required
			placeholder="password"
			v-model="password"
		/>
		<div class="error">
			{{error}}
		</div>
		<button>Sign up</button>
	</form>
</template>

<script>
	import {ref} from 'vue'
	import useLogin from '../composables/useLogin'
	export default {
		setup(props, context) {
			const email = ref('')
			const password = ref('')

			const { error, login } = useLogin()

			const handleSubmit = async () => {
				await login(email.value, password.value)
				if (!error.value) {
					console.log('use logged in')
					context.emit('login', true)
				}
			}

			return { email, password, handleSubmit, error }
		}
	}
</script>

<style scoped>

</style>
```

<br />

### Example #5

ChatWindow.vue

```vue
<template>
	<div class="chat-window">
		<div v-if="error">
			{{error}}
		</div>
		<div v-if="documents" class="messages" ref="containerRef">
			<div v-for="doc in formattedDocuments" :key="doc.id" class="single">
				<span class="created-at">{{doc.createdAt}}</span>
				<span class="name">{{doc.name}}</span>
				<span class="message">{{doc.message}}</span>
			</div>
		</div>
	</div>
</template>

<script>
	import getCollection from '../composables/getCollection'
	import {formatDistanceToNow} from 'date-fns'
	import {computed, onUpdated, ref} from 'vue'
	export default {
		setup() {
			const {error, documents} = getCollection('messages')
			const containerRef = ref(null)

			const formattedDocuments = computed(() => {
				if (documents.value) {
					return documents.value.map(doc => {
						let time = formatDistanceToNow(doc.createdAt.toDate())
						return {...doc, createdAt: time}
					})
				}
			})

			onUpdated(() => {
				containerRef.value.scrollTop = containerRef.value.scrollHeight;
			})

			return {error, documents, formattedDocuments, containerRef}
		}
	}
</script>
```

<br />

## Firebase config

### Firestore.rules

firestore.rules
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /messages/{messageId} {
      allow read, write: if request.auth != null;
    }
  }
}

```

<br />

### Firebase.json

firebase.json
```json
{
  "firestore": {
    "rules": "firestore.rules",
    "indexes": "firestore.indexes.json"
  },
  "hosting": {
    "public": "dist",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```

<br />

### .firebaserc

.firebaserc

```json
{
  "projects": {
    "default": "vue-firabase-udemy"
  }
}
```

### Firebase config

./firebase/config.js
```javascript
import firebase from 'firebase/app'
import 'firebase/firestore'
import 'firebase/auth'

const firebaseConfig = {
	apiKey: "AIzaSyCf7Cn6zEKLKVwzjkQVVA6EZvjGIK8ceeA",
	authDomain: "vue-firabase-udemy.firebaseapp.com",
	projectId: "vue-firabase-udemy",
	storageBucket: "vue-firabase-udemy.appspot.com",
	messagingSenderId: "513695555613",
	appId: "1:513695555613:web:7aaad12cea2cfe89d1a6f4"
};

// init firebase
firebase.initializeApp(firebaseConfig)

const projectAuth = firebase.auth()
const projectFirestore = firebase.firestore()
const timestamp = firebase.firestore.FieldValue.serverTimestamp

export {projectFirestore, projectAuth, timestam
```
