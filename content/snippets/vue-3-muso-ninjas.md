---
title: Vue 3 (muso ninjas)
---

## Set up

### package.json

package.json
```json
{
  "name": "muso-ninjas",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build"
  },
  "dependencies": {
    "core-js": "^3.6.5",
    "firebase": "^8.2.0",
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

<br />


## Main components 


App.vue
```vue
<template>
  <Navbar />
  <div class="content">
    <router-view/>
  </div>
</template>

<style>
  .content {
    margin: 0 auto;
    max-width: 1200px;
    padding: 0 20px;
  }
</style>
<script>
import Navbar from "./components/Navbar";
export default {
  components: {Navbar}
}
</script>
```

<br />

main.js
```javascript
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import {projectAuth} from "./firebase/config";
// global styles
import './assets/main.css'

let app
projectAuth.onAuthStateChanged(() => {
    if (!app) {
        app = createApp(App).use(router).mount('#app')
    }
})

```

<br />

## Views

Home.vue

```vue
<template>
  <div class="home">
    <div v-if="error" class="error">
      Coult not fetch the data
    </div>
    <div v-if="documents">
      <ListView :playlists="documents" />
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

import getCollection from "../composables/getCollection";
import ListView from "../components/ListView";

export default {
  name: 'Home',
  setup() {
    const {error, documents} = getCollection('playlists')
    console.log(documents)
    return {error, documents}
  },
  components: {
    ListView
  }
}
</script>
```

<br />

./playlists/CreatePlaylist.vue
```vue
<template>
  <form @submit.prevent="handleSubmit">
    <h4>Create New Playlist</h4>
    <input type="text" required placeholder="Playlist title" v-model="title" />
    <textarea required placeholder="Playlist description..." v-model="description"></textarea>
    <label>Upload playlist cover image</label>
    <input type="file" @change="handleChange">
    <div class="error">{{fileError}}</div>
    <div class="error"></div>
    <button v-if="!isPending">Create</button>
    <button v-else disabled>Saving...</button>
  </form>
</template>

<script>
import {ref} from "vue";
import useStorage from "../../composables/useStorage";
import useCollection from '../../composables/useCollection'
import getUser from "../../composables/getUser";
import {timestamp} from "../../firebase/config";
import {useRouter} from "vue-router";

export default {
  setup() {
    const title = ref('')
    const description = ref('')
    const file = ref(null)
    const fileError = ref(null)
    const isPending = ref(false)

    const {url, filePath, uploadImage} = useStorage()
    const {error, addDoc} = useCollection('playlists')
    const {user} = getUser()
    const router = useRouter()


    const handleSubmit = async () => {

      if (file.value) {
        isPending.value = true
        await uploadImage(file.value)
        const res = await addDoc({
          title: title.value,
          description: description.value,
          userId: user.value.uid,
          userName: user.value.displayName,
          coverUrl: url.value,
          filePath: filePath.value,
          songs: [],
          createdAt: timestamp()
        })
        isPending.value = false
        if (!error.value) {
          console.log('playlist added')
          await router.push({name: 'PlaylistDetails', params: {id: res.id}})
        }
      }
    }

    //allowed types
    const types = ['image/png', 'image/jpeg']

    const handleChange = (e) => {
      const selected = e.target.files[0]
      if (selected && types.includes(selected.type)) {
        file.value = selected
        fileError.value = null
      } else {
        file.value = null
        fileError.value = 'Please select and image file (png or jpg)'
      }
    }

    return {title, description, handleSubmit, handleChange, fileError, isPending}
  }
}
</script>
```

<br />

./playlists/PlaylistDetails.vue
```vue
<template>
  <h2>Playlist details {{id}}</h2>
  <div v-if="playlist" class="playlist-details">
    <div class="playlist-info">
      <div class="cover">
        <img :src="playlist.coverUrl" alt="no image"/>
      </div>
      <h2>
        {{playlist.title}}
      </h2>
      <p class="username">
        Created by {{playlist.userName}}
      </p>
      <p class="description">
        {{playlist.description}}
      </p>
      <button v-if="ownership" @click="handleDelete">
        Delete playlist
      </button>
    </div>
    <div class="song-list">
      <div v-if="!playlist.songs.length">
        No songs have been added to this playlist yet
      </div>
      <div v-for="song in playlist.songs" class="single-song" :key="song.id">
        <div class="details">
          <h3>{{song.title}}</h3>
          <p>{{song.artist}}</p>
        </div>
        <button v-if="ownership" @click="() => {handleDeleteSong(song.id)}">Delete</button>
      </div>
      <AddSong v-if="ownership" :playlist="playlist" />
    </div>
  </div>
  <div v-if="error" class="error">
    {{error}}
  </div>
</template>

<script>
import useDocument from "../../composables/useDocument";
import getDocument from "../../composables/getDocument";
import getUser from "../../composables/getUser";
import {computed} from "vue";
import useStorage from "../../composables/useStorage";
import {useRouter} from "vue-router";
import AddSong from "../../components/AddSong";

export default {
  components: {AddSong},
  setup(props) {
    const {document: playlist, error} = getDocument('playlists', props.id)
    const {deleteImage} = useStorage()
    const {user} = getUser()
    const {deleteDoc, updateDoc} = useDocument('playlists', props.id)
    const router = useRouter()
    const ownership = computed(() => {
      return playlist.value && user.value && user.value.uid === playlist.value.userId
    })
    const handleDelete = async () => {
      await deleteImage(playlist.value.filePath)
      const res = await deleteDoc('playlists', props.id)
      await router.push({name: 'Home'})
    }
    const handleDeleteSong = async (songId) => {
      const newSongs = playlist.value.songs.filter(song => {
        return song.id !== songId
      })
      await updateDoc({songs: newSongs})
    }
    return {playlist, error, ownership, handleDelete, handleDeleteSong}
  },
  props: {
    id: {
      type: Number,
      required: true
    }
  }
}
</script>
```

<br />


./playlists/UserPlaylists.vue

```vue
<template>
  <div class="user-playlists">
    <h2>My playlists</h2>
    <div v-if="playlists">
      <ListView :playlists="playlists"/>
    </div>
    <router-link :to="{ name: 'CreatePlaylist' }" class="btn"> Create playlist</router-link>
  </div>
</template>

<script>
import getUser from "../../composables/getUser";
import getCollection from "../../composables/getCollection";
import ListView from "../../components/ListView";

export default {
  setup() {
    const {user} = getUser()
    const {documents: playlists} = getCollection('playlists',
        ['userId', '==', user.value.uid])

    console.log(playlists)

    return {playlists}
  },
  components: {
    ListView
  }
}
</script>
```

<br />

./auth/Login.vue
```vue
<template>
	<form @submit.prevent="handleSubmit">
		<h3>Login form</h3>
		<input type="email" placeholder="Email" v-model="email"/>
		<input type="password" placeholder="password" v-model="password"/>
    <div v-if="error" class="error"> {{error}}</div>
		<button v-if="!isPending">Log in</button>
		<button v-if="isPending" disabled>Loading</button>
	</form>
</template>

<script>
	import {ref} from 'vue'
	import useLogin from '../../composables/useLogin'
  import {useRouter} from "vue-router";


	export default {
		setup() {
			const {error, login, isPending} = useLogin()
			const email = ref('')
			const password = ref('')
      const router = useRouter()

			const handleSubmit = async () => {
				const res = await login(email.value, password.value)
        if (!error.value) {
          await router.push({name: 'UserPlaylist'})
        }
			}

			return {email, password, handleSubmit, error, isPending}
		}
	}
</script>
```

<br />

./auth/Signup.vue
```vue
<template>
  <form @submit.prevent="handleSubmit">
    <h3>Login form</h3>
    <input type="text" placeholder="display name" v-model="displayName"/>
    <input type="email" placeholder="Email" v-model="email"/>
    <input type="password" placeholder="password" v-model="password"/>
    <div v-if="error" class="error"> {{error}}</div>
    <button v-if="!isPending">Log in</button>
    <button v-if="isPending" disabled>Loading</button>
  </form>
</template>

<script>
import {ref} from 'vue'
import useSignup from "../../composables/useSignup";
import {useRouter} from "vue-router";


export default {
  setup() {
    const {error, signup, isPending} = useSignup()
    const email = ref('')
    const displayName = ref('')
    const password = ref('')
    const router = useRouter()

    const handleSubmit = async () => {
      const res = await signup(email.value, password.value, displayName.value)
      if (!error.value) {
        await router.push({name: 'UserPlaylist'})
      }
    }

    return {email, password, displayName, handleSubmit, error, isPending}
  }
}
</script>
```
<br />


## Router

./index.js
```javascript
import { createRouter, createWebHistory } from 'vue-router'
import Home from '../views/Home.vue'
import Login from '../views/auth/Login'
import Signup from "../views/auth/Signup";
import CreatePlaylist from "../views/playlists/CreatePlaylist";
import {projectAuth} from "../firebase/config";
import PlaylistDetails from "../views/playlists/PlaylistDetails";
import UserPlaylists from "../views/playlists/UserPlaylists";

const requireAuth = (to, from, next) => {
  let user = projectAuth.currentUser
  if (!user) {
    next({name: 'Login'})
  } else {
    next()
  }
}

const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home,
    beforeEnter: requireAuth
  },
  {
    path: '/login',
    name: 'Login',
    component: Login
  },
  {
    path: '/signup',
    name: 'Signup',
    component: Signup
  },
  {
    path: '/playlist/create',
    name: 'CreatePlaylist',
    component: CreatePlaylist,
    beforeEnter: requireAuth
  },
  {
    path: '/playlists/user',
    name: 'UserPlaylist',
    component: UserPlaylists,
    beforeEnter: requireAuth
  },
  {
    path: '/playlist/:id',
    name: 'PlaylistDetails',
    component: PlaylistDetails,
    beforeEnter: requireAuth,
    props: true
  },
]

const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes
})

export default router
```

<br />

## Composables

getCollection.js
```javascript
import { ref, watchEffect } from 'vue'
import { projectFirestore } from '../firebase/config'

const getCollection = (collection, query) => {

  const documents = ref(null)
  const error = ref(null)

  // register the firestore collection reference
  let collectionRef = projectFirestore.collection(collection)
    .orderBy('createdAt')

  if (query) {
    collectionRef = collectionRef.where(...query)
  }

  const unsub = collectionRef.onSnapshot(snap => {
    let results = []
    snap.docs.forEach(doc => {
      // must wait for the server to create the timestamp & send it back
      doc.data().createdAt && results.push({...doc.data(), id: doc.id})
    });
    
    // update values
    documents.value = results
    error.value = null
  }, err => {
    console.log(err.message)
    documents.value = null
    error.value = 'could not fetch the data'
  })

  watchEffect((onInvalidate) => {
    onInvalidate(() => unsub());
  });

  return { error, documents }
}

export default getCollection
```

<br />

getDocument.js
```javascript
import { ref, watchEffect } from 'vue'
import { projectFirestore } from '../firebase/config'

const getDocument = (collection, id) => {

    const document = ref(null)
    const error = ref(null)

    // register the firestore collection reference
    let documentRef = projectFirestore.collection(collection).doc(id)

    const unsub = documentRef.onSnapshot(doc => {
        if (doc.data()) {
            document.value = {...doc.data(), id: doc.id}
            error.value = null
        } else {
            error.value = 'that document does not exist'
        }
        // update values
    }, err => {
        console.log(err.message)
        document.value = null
        error.value = 'could not fetch the data'
    })

    watchEffect((onInvalidate) => {
        onInvalidate(() => unsub());
    });

    return { error, document }
}

export default getDocument
```

<br />

getUser.js
```javascript
import { ref } from 'vue'
import { projectAuth } from '../firebase/config'

// refs
const user = ref(projectAuth.currentUser)

// auth changes
projectAuth.onAuthStateChanged(_user => {
  console.log('User state change. Current user is:', _user)
  user.value = _user
});

const getUser = () => {
  return { user } 
}

export default getUser
```

<br />

useCollection.js
```javascript
import { ref } from 'vue'
import { projectFirestore } from '../firebase/config'

const useCollection = (collection) => {

  const error = ref(null)
  const isPending = ref(false)

  // add a new document
  const addDoc = async (doc) => {
    error.value = null
    isPending.value = true

    try {
      const res = await projectFirestore.collection(collection).add(doc)
      isPending.value = false
      return res
    }
    catch(err) {
      console.log(err.message)
      isPending.value = false
      error.value = 'could not send the message'
    }
  }

  return { error, addDoc, isPending}

}

export default useCollection
```

<br />

useDocument.js

```javascript
import {ref} from 'vue'
import {projectFirestore} from "../firebase/config";

const useDocument = (collection, id) => {
    const error = ref(null)
    const isPending = ref(false)

    let docRef = projectFirestore.collection(collection).doc(id)

    const deleteDoc = async () => {
        isPending.value = true
        error.value = null
        try {
            const res = await docRef.delete()
            isPending.value = false
            error.value = false
            return res
        } catch (err) {
            console.log(err.message)
            isPending.value = false
            error.value = 'Could not delete the document'
        }
    }

    const updateDoc = async (updates) => {
        isPending.value = true
        error.value = null
        try {
            const res = await docRef.update(updates)
            isPending.value = false
            error.value = false
            return res
        } catch (err) {
            console.log(err.message)
            isPending.value = false
            error.value = 'Could not update the document'
        }
    }

    return {error, isPending, deleteDoc, updateDoc}
}

export default useDocument
```

<br />

useLogin.js
```javascript
import { ref } from 'vue'
import { projectAuth } from '../firebase/config'

const error = ref(null)
const isPending = ref(false)

const login = async (email, password) => {
  error.value = null
  isPending.value = true

  try {
    const res = await projectAuth.signInWithEmailAndPassword(email, password)
    error.value = null
    isPending.value = false
    return res
  }
  catch(err) {
    console.log(err.message)
    error.value = 'Incorrect login credentials'
    isPending.value = false
  }
}

const useLogin = () => {
  return { error, login, isPending }
}

export default useLogin
```

<br />

useLogout.js
```javascript
import { ref } from 'vue'
import { projectAuth } from '../firebase/config'

// refs
const error = ref(null)
const isPending = ref(false)


// logout function
const logout = async () => {
  error.value = null
  isPending.value = true

  try {
    await projectAuth.signOut()
    isPending.value = false
  }
  catch(err) {
    console.log(err.message)
    error.value = err.message
    isPending.value = false
  }
}

const useLogout = () => {
  return { error, logout, isPending }
}

export default useLogout
```

<br />

useSignup.js
```javascript
import { ref } from 'vue'
import { projectAuth } from '../firebase/config'

const error = ref(null)
const isPending = ref(false)

const signup = async (email, password, displayName) => {
  error.value = null
  isPending.value = true

  try {
    const res = await projectAuth.createUserWithEmailAndPassword(email, password)
    if (!res) {
      throw new Error('Could not complete signup')
    }
    await res.user.updateProfile({ displayName })
    isPending.value = false
    error.value = null
    
    return res
  }
  catch(err) {
    console.log(err.message)
    error.value = err.message
    isPending.value = false
  }
}

const useSignup = () => {
  return { error, signup, isPending }
}

export default useSignup
```

<br />


useStorage.js
```javascript
import {projectStorage} from "../firebase/config";
import getUser from "./getUser";
import {ref} from 'vue'

const {user} = getUser()

const useStorage = () => {
    const error = ref(null)
    const url = ref(null)
    const filePath = ref(null)

    const uploadImage = async (file) => {
        filePath.value = `covers/${user.value.uid}/${file.name}`
        const storageRef = projectStorage.ref(filePath.value)

        try {
            const res = await storageRef.put(file)
            url.value = await res.ref.getDownloadURL()
        } catch (err) {
            console.log(err.message)
            error.value = err.message
        }
    }

    const deleteImage = async (path) => {
        const storageRef = projectStorage.ref(path)
        try {
            await storageRef.delete()
        } catch(err) {
            console.log(err.message)
            error.value = err.message
        }
    }

    return {url, filePath, error, uploadImage, deleteImage}
}

export default useStorage
```

<br />

## Reusable components

AddSong.vue
```vue
<template>
  <div class="add-song">
    <button v-if="!showForm" @click="showForm = true">Add songs</button>
    <form v-if="showForm" @submit.prevent="handleSubmit">
      <h4>Add a new song</h4>
      <input type="text" placeholder="Song title" required v-model="title"/>
      <input type="text" placeholder="Artist" required v-model="artist"/>
      <button>Add</button>
    </form>
  </div>
</template>

<script>
import {ref} from 'vue'
import useDocument from "../composables/useDocument";

export default {
  props: {
    playlist: {
      type: Object,
      required: true
    }
  },
  setup(props) {
    const title = ref('')
    const artist = ref('')
    const showForm = ref(false)
    const {updateDoc} = useDocument('playlists', props.playlist.id)

    const handleSubmit = async () => {
      const newSong = {
        title: title.value,
        artist: artist.value,
        id: Math.floor(Math.random() * 1000000)
      }
      await updateDoc({
        songs: [...props.playlist.songs, newSong]
      })
      title.value = ''
      artist.value = ''
    }

    return {title, artist, showForm, handleSubmit}
  }
}
</script>
```

<br />

ListView.vue

```vue
<template>
  <div v-for="playlist in playlists" :key="playlist.id">
    <router-link :to="{ name: 'PlaylistDetails', params: {id: playlist.id}}">
      <div class="single">
        <div class="thumbnail">
          <img :src="playlist.coverUrl"/>
        </div>
        <div class="info">
          <h3>{{playlist.title}}</h3>
          <p>Created by {{playlist.userName}}</p>
        </div>
        <div class="song-number">
          <p>{{playlist.songs.length}}</p>
        </div>
      </div>
    </router-link>
  </div>
</template>

<script>
export default {
  props: {
    playlists: {
      type: Array,
      required: true
    }
  }
}
</script>
```

<br />

Navbar.vue
```vue
<template>
  <div class="navbar">
    <nav>
      <img src="../assets/ninja.png" alt=""/>
      <h1>
        <router-link :to="{name: 'Home'}">Muso Ninjas</router-link>
      </h1>
      <div class="links">
        <div v-if="user">
          <router-link :to="{name: 'CreatePlaylist'}">Create playlist</router-link>
          <router-link :to="{name: 'UserPlaylist'}">My playlist</router-link>
          <span>Hi there, {{user.displayName}}</span>
          <button @click="handleSubmit">Logout</button>
          <button v-if="isPending" disabled>Waiting...</button>
        </div>
        <div v-if="!user">
          <router-link class="btn" :to="{name: 'Signup'}">Signup</router-link>
          <router-link class="btn" :to="{name: 'Login'}">Log in</router-link>
        </div>
      </div>
    </nav>
  </div>
</template>

<script>
import useLogout from "../composables/useLogout";
import {useRouter} from "vue-router";
import getUser from "../composables/getUser";

export default {

  setup() {
    const {logout, error, isPending} = useLogout()
    const {user} = getUser()
    const router = useRouter()
    const handleSubmit = async () => {
      const res = await logout()
      if (!error.value) {
        await router.push({name: 'Login'})
      }
    }

    return {handleSubmit, isPending, user}
  }
}
</script>
```


<br />


## Extra

### Firebase

storage.rules
```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /covers/{userId}/{document=**} {
      allow read, create: if request.auth != null;
      allow delete: if request.auth.uid != userId;
    }
  }
}
```

<br />

firestore.rules
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /playlists/{docId} {
      allow read, create: if request.auth != null;
      allow delete, update: if request.auth.uid == resource.data.userId;
    }
  }
}
```

<br />

firestore.indexes.json
```json
{
  "indexes": [],
  "fieldOverrides": []
}

```


<br />

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
  },
  "storage": {
    "rules": "storage.rules"
  }
}
```

<br />

.firebaserc
```
{
  "projects": {
    "default": "muso-ninjas-2f9f9"
  }
}
```

<br />


./firebase/config.js

```javascript
import firebase from 'firebase/app'
import 'firebase/firestore'
import 'firebase/auth'
import 'firebase/storage'

const firebaseConfig = {
	apiKey: "AIzaSUEi2rLk__6dL0Gv9VC4FqFQ",
	authDomain: "muso-ninjas-f9.firebaseapp.com",
	projectId: "muso-ninjas-2f9f9",
	storageBucket: "muso-ninjas-f9.appspot.com",
	messagingSenderId: "9866264",
	appId: "1:986626753ec91ecf6363"
};

// init firebase
firebase.initializeApp(firebaseConfig)

const projectFirestore = firebase.firestore()
const projectAuth = firebase.auth()
const projectStorage = firebase.storage()

//timestamp

const timestamp = firebase.firestore.FieldValue.serverTimestamp

export { projectFirestore, projectAuth, projectStorage, timestamp}
```
