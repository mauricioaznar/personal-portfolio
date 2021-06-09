---
title: React techniques
---


## Introduction

React is an open-source, front end, JavaScript library for building user interfaces or
UI components. It is maintained by Facebook and a community of individual 
developers and companies. React can be used as a base in the development 
of single-page or mobile applications. ([react docs](https://reactjs.org/docs/getting-started.html))


<br />

## Components


### prop-types

TodoList.jsx

```jsx
import React from 'content/snippets/react-techniques'
import PropTypes from 'prop-types'
import Todo from './Todo'

const TodoList = ({ todos, onTodoClick }) => {
  return <ul>
    {todos.map((todo, index) => {
      return <Todo
        key={todo.id} {...todo}
        onClick={() => onTodoClick(todo)}
      />
    })}
  </ul>
}

TodoList.propTypes = {
  todos: PropTypes.arrayOf(PropTypes.shape({
      id: PropTypes.number.isRequired,
      completed: PropTypes.bool.isRequired,
      text: PropTypes.string.isRequired
    }).isRequired
  ).isRequired,
  onTodoClick: PropTypes.func.isRequired
}

export default TodoList
```

<br />

Todo.js

```jsx
import React from 'content/snippets/react-techniques'
import PropTypes from 'prop-types'

const Todo = ({ onClick, completed, text }) => {
  return <li
    onClick={() => onClick()}
    style={{
      textDecoration: completed ? 'line-through' : 'none'
    }}
  >
    {text}
  </li>
}

Todo.propTypes = {
  onClick: PropTypes.func.isRequired,
  completed: PropTypes.bool.isRequired,
  text: PropTypes.string.isRequired
}

export default Todo
```

<br />

### HOC (high-order component)

LogHoc.js

```jsx
import React from 'content/snippets/react-techniques'

function logHoc(WrappedComponent) {
  return class extends React.Component {
    render() {
      return <WrappedComponent {...this.props} >
        {this.children}
      </WrappedComponent>;
    }
  }
}

export default logHoc
```

<br />


## Redux

### Connect a component

Header.js

```javascript
import React, { Component } from 'content/snippets/react-techniques';
import { connect } from 'react-redux'
import { Link } from "react-router-dom";
import Payments from "./Payments";

class Header extends Component {
  renderContent() {
    switch (this.props.auth) {
      case null:
        return
      case false:
        return (
          <li>
            <a href="/auth/google">Login with google</a>
          </li>
        )
      default:
        return [
          <li key="1">
            <Payments />
          </li>,
          <li key="3" style={{ margin: '0 10px' }}>
            Credits: {this.props.auth.credits}
          </li>,
          <li key="2">
            <a href='/api/logout'>Logout</a>
          </li>
        ];

    }
  }

  render() {
    return (
      <nav className="nav-wrapper">
        <Link
          to={this.props.auth ? '/surveys' : '/'}
          className="left brand-logo"
        >
          Emaily
        </Link>
        <ul className="right">
          {this.renderContent()}
        </ul>
      </nav>
    );
  }
}

function mapStateToProps(state) {
  return {
    auth: state.auth
  }
}

export default connect(mapStateToProps, null)(Header);
```

<br />

Link.js

```javascript
import React from 'content/snippets/react-techniques'
import { connect } from 'react-redux'
import { setVisibilityFilter } from '../actions'
import Link from '../components/Link'

const mapStateToProps = (state, ownProps) => {
  return { active: state.visibilityFilter === ownProps.filter }
}

const mapDispatchToProps = (dispatch, ownProps) => {
  return {
    onClick: () => {
      dispatch(setVisibilityFilter(ownProps.filter))
    }
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(Link)
```

<br />

### Complex actions example #1

auth.js
```javascript
import axios from 'axios'
import {setAlert} from "./alert";
import {
  REGISTER_FAIL,
  REGISTER_SUCCESS,
  USER_LOADED,
  AUTH_ERROR,
  LOGIN_SUCCESS,
  LOGIN_FAIL,
  LOGOUT,
  CLEAR_PROFILE
} from "./types";
import setAuthToken from "../utils/setAuthToken";

// Load User
export const loadUser = () => async dispatch => {
  if (localStorage.token) {
    setAuthToken(localStorage.token)
  }

  try {
    const res = await axios.get('/api/auth')
    dispatch({
      type: USER_LOADED,
      payload: res.data
    })
  } catch (err) {
    console.log(err.response)
    dispatch({
      type: AUTH_ERROR
    })
  }
}

// Register User
export const register = ({name, email, password}) => async dispatch => {
  const config = {
    headers: {
      'Content-Type': 'application/json'
    }
  }

  const body = JSON.stringify({name, email, password})

  try {
    const res = await axios.post('/api/users', body, config)
    dispatch({
      type: REGISTER_SUCCESS,
      payload: res.data
    })
    dispatch(loadUser())
  } catch (err) {
    const errors = err.response.data.errors
    if (errors) {
      errors.forEach(error => dispatch(setAlert(error.msg, 'danger')))
    }

    dispatch({
      type: REGISTER_FAIL
    })
  }
}

// Login User
export const login = (email, password) => async dispatch => {
  const config = {
    headers: {
      'Content-Type': 'application/json'
    }
  }

  const body = JSON.stringify({email, password})

  try {
    const res = await axios.post('/api/auth', body, config)

    dispatch({
      type: LOGIN_SUCCESS,
      payload: res.data
    })

    dispatch(loadUser())
  } catch (err) {
    const errors = err.response.data.errors
    if (errors) {
      errors.forEach(error => dispatch(setAlert(error.msg, 'danger')))
    }

    dispatch({
      type: LOGIN_FAIL
    })
  }
}

// Logout / Clear profile

export const logout = () => dispatch => {
  console.log('logout')
  dispatch({type: CLEAR_PROFILE})
  dispatch({type: LOGOUT})
}
```

<br />

### Complex actions example #2

post.js
```javascript
import axios from 'axios'
import {setAlert} from './alert'
import {
  ADD_COMMENT,
  ADD_POST,
  DELETE_POSTS,
  GET_POST,
  GET_POSTS,
  POST_ERROR,
  REMOVE_COMMENT,
  UPDATE_LIKES,
} from "./types";

export const getPosts = () => async dispatch => {
  try {
    const res = await axios.get('/api/posts')

    dispatch({
      type: GET_POSTS,
      payload: res.data
    })
  } catch (err) {
    dispatch({
      type: POST_ERROR,
      payload: {msg: err.response.statusText, status: err.response.status}
    })
  }
}

export const addLike = (postId) => async dispatch => {
  try {
    const res = await axios.put(`/api/posts/like/${postId}`)

    dispatch({
      type: UPDATE_LIKES,
      payload: {postId, likes: res.data}
    })
  } catch (err) {
    dispatch({
      type: POST_ERROR,
      payload: {msg: err.response.statusText, status: err.response.status}
    })
  }
}

export const removeLike = (postId) => async dispatch => {
  try {
    const res = await axios.put(`/api/posts/unlike/${postId}`)

    dispatch({
      type: UPDATE_LIKES,
      payload: {postId, likes: res.data}
    })
  } catch (err) {
    dispatch({
      type: POST_ERROR,
      payload: {msg: err.response.statusText, status: err.response.status}
    })
  }
}

export const deletePost = postId => async dispatch => {
  try {
    const res = await axios.delete(`/api/posts/${postId}`)

    dispatch({
      type: DELETE_POSTS,
      payload: postId
    })
    dispatch(setAlert('Post Removed', 'success'))
  } catch (err) {
    dispatch({
      type: POST_ERROR,
      payload: {msg: err.response.statusText, status: err.response.status}
    })
  }
}

export const addPost = formData => async dispatch => {
  const config = {
    headers: {
      'Content-Type': 'application/json'
    }
  }

  try {
    const res = await axios.post(`/api/posts`, formData, config)

    dispatch({
      type: ADD_POST,
      payload: res.data
    })
    dispatch(setAlert('Post Created', 'success'))
  } catch (err) {
    dispatch({
      type: POST_ERROR,
      payload: {msg: err.response.statusText, status: err.response.status}
    })
  }
}

export const getPost = postId => async dispatch => {
  try {
    const res = await axios.get(`/api/posts/${postId}`)

    dispatch({
      type: GET_POST,
      payload: res.data
    })

  } catch (err) {
    dispatch({
      type: POST_ERROR,
      payload: {msg: err.response.statusText, status: err.response.status}
    })
  }
}

export const addComment = (postId, formData) => async dispatch => {
  const config = {
    headers: {
      'Content-Type': 'application/json'
    }
  }

  try {
    const res = await axios.post(`/api/posts/comment/${postId}`, {text: formData}, config)

    dispatch({
      type: ADD_COMMENT,
      payload: res.data
    })
    dispatch(setAlert('Comment added', 'success'))
  } catch (err) {
    dispatch({
      type: POST_ERROR,
      payload: {msg: err.response.statusText, status: err.response.status}
    })
  }
}

export const deleteComment = (postId, commentId) => async dispatch => {

  try {
    const res = await axios.delete(`/api/posts/comment/${postId}/${commentId}`)

    dispatch({
      type: REMOVE_COMMENT,
      payload: commentId
    })
    dispatch(setAlert('Comment removed!', 'success'))
  } catch (err) {
    dispatch({
      type: POST_ERROR,
      payload: {msg: err.response.statusText, status: err.response.status}
    })
  }
}
```

<br />

### Complex reducer example #1

auth.js
```javascript
import {
  REGISTER_FAIL,
  REGISTER_SUCCESS,
  USER_LOADED,
  AUTH_ERROR,
  LOGIN_SUCCESS,
  LOGIN_FAIL,
  LOGOUT,
  ACCOUNT_DELETED
} from "../actions/types";

const initialState = {
  token: localStorage.getItem('token'),
  isAuthenticated: null,
  loading: true,
  user: null
}

export default function auth (state = initialState, action) {
  const {type, payload} = action

  switch(type) {
    case USER_LOADED:
      return {
        ...state,
        user: payload,
        isAuthenticated: true,
        loading: false,
      }
    case LOGIN_SUCCESS:
    case REGISTER_SUCCESS:
      localStorage.setItem('token', payload.token)
      return {
        ...state,
        ...payload,
        isAuthenticated: true,
        loading: false
      }
    case AUTH_ERROR:
    case LOGIN_FAIL:
    case REGISTER_FAIL:
    case LOGOUT:
    case ACCOUNT_DELETED:
      localStorage.removeItem('token')
      return {
        ...state,
        token: null,
        isAuthenticated: false,
        loading: false
      }
    default:
      return state
  }
}
```
<br />

### Complex reducer example #2

post.js
```javascript
import {
  DELETE_POSTS,
  GET_POSTS,
  POST_ERROR,
  UPDATE_LIKES,
  ADD_POST,
  GET_POST,
  ADD_COMMENT,
  REMOVE_COMMENT
} from "../actions/types";

const initialState = {
  posts: [],
  post: null,
  loading: true,
  error: {}
}

export default function post (state = initialState, action) {
  const {type, payload} = action

  switch (type) {
    case GET_POSTS:
      return {
        ...state,
        posts: payload,
        loading: false
      }
    case POST_ERROR:
      return {
        ...state,
        error: payload,
        loading: false
      }
    case UPDATE_LIKES:
      return {
        ...state,
        posts: state.posts.map(post => {
          if (post._id === payload.postId) {
            return {...post, likes: payload.likes}
          } else {
            return post
          }
        }),
        loading: false
      }
    case ADD_POST:
      return {
        ...state,
        posts: [payload, ...state.posts],
        loading: false
      }
    case GET_POST:
      return {
        ...state,
        post: payload,
        loading: false
      }
    case DELETE_POSTS:
      return {
        ...state,
        posts: state.posts.filter(post => {
          return post._id !== payload
        }),
        loading: false
      }
    case ADD_COMMENT:
      return {
        ...state,
        post: {...state.post, comments: payload},
        loading: false
      }
    case REMOVE_COMMENT:
      return {
        ...state,
        post: {
          ...state.post,
          comments: state.post.comments.filter(comment => {
            return comment._id !== payload
          }),
          loading: false
        }
      }
    default:
      return state
  }
}
```

<br />

### Add devTools

```javascript
import {createStore, applyMiddleware} from 'redux'
import {composeWithDevTools} from "redux-devtools-extension";
import thunk from 'redux-thunk'
import rootReducer from './reducers'

const initialState = {}

const middleware = [thunk]

const store = createStore(
  rootReducer,
  initialState,
  composeWithDevTools(applyMiddleware(...middleware)))

export default store
```

<br />

## Libraries

### Stripe.js 
Dependency used `"react-stripe-checkout": "^2.6.3",`. Following is an example about how to use
the library.

Payments.js

```jsx
import React, { Component } from 'content/snippets/react-techniques';
import StripeCheckout from 'react-stripe-checkout'
import { connect } from 'react-redux'
import * as actions from '../actions'

class Payments extends Component {
  render() {
    return (
      <StripeCheckout
        name="Emaily"
        description="$5 for 5 emaily credits"
        amount={500}
        token={(token => this.props.handleToken(token))}
        stripeKey={process.env.REACT_APP_STRIPE_KEY}
      >
        <button className="btn">Add credits</button>
      </StripeCheckout>
    );
  }
}

export default connect(null, actions)(Payments);
```

<br />

### Redux form
Dependency used `"react-redux": "^7.2.2",`

reducer.js
```javascript
import {combineReducers} from "redux";
import {reducer as reduxForm} from 'redux-form';
import authReducer from "./authReducer";
import surveysReducer from "./surveysReducer";

export default combineReducers({
  auth: authReducer,
  surveys: surveysReducer,
  form: reduxForm
})
```

<br />


SurveyNew.js

```jsx
import React, { Component } from 'content/snippets/react-techniques';
import SurveyForm from "./SurveyForm";
import SurveyFormReview from './SurveyFormReview'
import { reduxForm } from "redux-form";

class SurveyNew extends Component {

  state = {
    showFormReview: false
  }

  renderContent() {
    if (this.state.showFormReview === true) {
      return <SurveyFormReview
        onCancel={() => this.setState({ showFormReview: false })}
      />
    } else {
      return <SurveyForm
        onSurveySubmit={() => this.setState({ showFormReview: true })}
      />
    }
  }

  render() {
    return (
      <div>
        {this.renderContent()}
      </div>
    );
  }
}

export default reduxForm({
  form: 'surveyForm'
})(SurveyNew);
```

<br />

SurveyForm.js

```jsx
import React, { Component } from 'content/snippets/react-techniques';
import { reduxForm, Field } from "redux-form";
import SurveyField from "./SurveyField";
import _ from 'lodash'
import { Link } from 'react-router-dom'
import validateEmails from "../../utils/validateEmails";
import formFields from './formFields'

class SurveyForm extends Component {

  renderFields() {
    return (
      _.map(formFields, ({ label, name }) => {
        return <Field
          key={name}
          component={SurveyField}
          type="text"
          label={label}
          name={name}
        />
      })
    )
  }

  render() {
    return (
      <div>
        <form onSubmit={this.props.handleSubmit(() => this.props.onSurveySubmit())}>
          {this.renderFields()}
          <Link
            to="/surveys"
            className="red btn-flat white-text"
          >
            Cancel
          </Link>
          <button
            type="submit"
            className="teal btn-flat right white-text"
          >
            Next
            <i className="material-icons right">done</i>
          </button>
        </form>
      </div>
    );
  }
}

function validate(values) {
  const errors = {}

  _.each(formFields, ({ name }) => {
    if (!values[name]) {
      errors[name] = 'You must provide a value'
    }
  })

  errors.recipients = validateEmails(values.recipients || '')

  return errors
}

export default reduxForm({
  validate,
  form: 'surveyForm',
  destroyOnUnmount: false
})(SurveyForm);
```

<br />


SurveyField.js

```jsx
import React from 'content/snippets/react-techniques';

const SurveyField = ({ input, label, meta: { error, touched } }) => {
  return (
    <div>
      <label>{label}</label>
      <input {...input} style={{ marginBottom: '5px' }} />
      <div
        className="red-text"
        style={{ marginBottom: '20px' }}
      >
        {touched && error}
      </div>
    </div>
  );
};

export default SurveyField;
```

<br />

formFields.js

```javascript
export default  [
  {label: 'Survey title', name: 'title'},
  {label: 'Subject line', name: 'subject'},
  {label: 'Email body', name: 'body'},
  {label: 'Recipient list', name: 'recipients'}
]
```

<br />

SurveyReview.js

```jsx
import React from 'content/snippets/react-techniques';
import { connect } from 'react-redux'
import formFields from './formFields'
import _ from 'lodash'
import { withRouter } from 'react-router-dom'
import * as actions from '../../actions'

const SurveyFormReview = ({ onCancel, formValues, submitSurvey, history }) => {

  const reviewFields = _.map(formFields, ({ label, name }) => {
    return (
      <div key={name}>
        <label>{label}</label>
        <div>
          {formValues[name]}
        </div>
      </div>
    )
  })

  return (
    <div>
      <h5>
        Please confirm you entries
      </h5>
      {reviewFields}
      <button
        className="yellow darken-3 white-text btn-flat"
        onClick={onCancel}
      >
        Back
      </button>
      <button
        onClick={() => submitSurvey(formValues, history)}
        className="green white-text btn-flat right"
      >
        Send Survey
        <i className="material-icons right">email</i>
      </button>
    </div>
  );
};

function mapStateToProps(state) {
  return {
    formValues: state.form.surveyForm.values
  }
}

export default connect(mapStateToProps, actions)(withRouter(SurveyFormReview));
```

<br />

## Routing

### Private route

PrivateRoute.jsx
```jsx
import React from 'react';
import {Route, Redirect} from 'react-router-dom'
import PropTypes from 'prop-types';
import {connect} from 'react-redux'

PrivateRoute.propTypes = {
  auth: PropTypes.object.isRequired
};

function PrivateRoute({ component: Component, ...rest }) {
  return <Route
    {...rest}
    render={
      props => !rest.auth.isAuthenticated && !rest.auth.loading
        ? <Redirect to="/login" />
        : <Component {...props} />
    }
  />
}

const mapStateToProps = (state) => {
  return {
    auth: state.auth
  }
}

export default connect(mapStateToProps, null)(PrivateRoute);
```

<br />

App.js
```jsx
import React from 'react'
import {BrowserRouter as Router, Switch, Route} from 'react-router-dom'
import Navbar from './components/layout/Navbar'
import Landing from './components/layout/Landing'
import Login from './components/auth/Login'
import Register from './components/auth/Register'
import Dashboard from './components/dashboard/Dashboard'
import Alert from './components/layout/Alert'
import './App.css'
import { Provider } from 'react-redux'
import store from './store'
import setAuthToken from "./utils/setAuthToken";
import {loadUser} from "./actions/auth";
import PrivateRoute from "./components/routing/PrivateRoute";

if (localStorage.token) {
  setAuthToken(localStorage.token)
}

const App = () => {

  React.useEffect(() => {
    store.dispatch(loadUser())
  }, [])

  return (
    <Provider store={store}>
      <Router>
        <React.Fragment>
          <Route exact path="/" component={Landing}/>
          <Navbar />
          <section className="container">
            <Alert />
            <Switch>
              <Route exact path="/register" component={Register} />
              <PrivateRoute exact path="/dashboard" component={Dashboard} />
            </Switch>
          </section>
        </React.Fragment>
      </Router>
    </Provider>
  );
}

export default App;

```

### Change value depending on path (watch path)

```javascript
const menuOptions = [
  {name: 'Services', link: '/services', activeIndex: 1, selectedIndex: 0},
  {name: 'Custom software development', link: '/customsoftware',  activeIndex: 1, selectedIndex: 1},
  {name: 'iOS/Android app development ', link: '/mobileapps', activeIndex: 1, selectedIndex: 2},
  {name: 'Websites development', link: '/websites', activeIndex: 1, selectedIndex: 3}
]

const routes = [
  {name: 'Home', link: '/', activeIndex: 0},
  {name: 'Services', link: '/services',  activeIndex: 1, ariaOwns: (anchorEl ? 'simple-menu' : undefined), ariaHasPopup: (anchorEl ? 'true' : undefined), onMouseOver: (e) => { handleClick(e) }},
  {name: 'The Revolution', link: '/revolution',  activeIndex: 2},
  {name: 'About us', link: '/about',  activeIndex: 3},
  {name: 'Contact', link: '/contact',  activeIndex: 4}
]
const {value, setValue, selectedIndex, setSelectedIndex} = props
useEffect(() => {
  [...menuOptions, ...routes].forEach(route => {
    switch (window.location.pathname) {
      case `${route.link}`:
        if (value !== route.activeIndex) {
          setValue(route.activeIndex)
          if (route.selectedIndex && route.selectedIndex !== selectedIndex) {
            setSelectedIndex(route.selectedIndex)
          }
        }
        break
      default:
        break;
    }
  })
}, [value, menuOptions, routes, selectedIndex, setSelectedIndex, setValue])
```


## Animations

### React-Lottie

Dependency used `"react-lottie": "^1.2.3",`.

1. Export from adobe after effects using bodymovin plugin
2. Format exported json like the following.

documentAnimation.js
```javascript
import img0 from "./img_0.png";
import img1 from "./performance.png";

export default {
  v: "5.5.9",
  fr: 29.9700012207031,
  ip: 0,
  op: 180.00000733155,
  w: 636,
  h: 646,
  nm: "Digitail Documents and Data Animation",
  ddd: 0,
  assets: [
    { id: "image_0", w: 372, h: 492, u: "", p: img0, e: 0 },
    { id: "image_1", w: 604, h: 317, u: "", p: img1, e: 0 },
    {
      id: "comp_0",
      layers: [
        {
          ddd: 0,
          ind: 1,
          ty: 2,
          nm: "Papers",
          refId: "image_0",
          sr: 1,
          ks: {
            o: { a: 0, k: 100, ix: 11 },
            r: { a: 0, k: 0, ix: 10 },
            p: { a: 0, k: [971, 326.5, 0], ix: 2 },
            a: { a: 0, k: [185.75, 245.75, 0], ix: 1 },
            s: {
              a: 1,
              k: [
                {
                  i: { x: [0, 0, 0.667], y: [1, 1, 1] },
                  o: { x: [0.333, 0.333, 0.333], y: [0, 0, 0] },
                  t: 30,
                  s: [0, 0, 100]
                },
                {
                  i: { x: [0.667, 0.667, 0.667], y: [1, 1, 1] },
                  o: { x: [0.333, 0.333, 0.333], y: [0, 0, 0] },
                  t: 90,
                  s: [100, 100, 100]
                },
                {
                  i: { x: [0, 0, 0.667], y: [1, 1, 1] },
                  o: { x: [0.736, 0.736, 0.333], y: [0, 0, 0] },
                  t: 135,
                  s: [100, 100, 100]
                },
                { t: 180.00000733155, s: [0, 0, 100] }
              ],
              ix: 6
            }
          },
          ao: 0,
          ip: 0,
          op: 180.00000733155,
          st: 0,
          bm: 0
        },
        {
          ddd: 0,
          ind: 2,
          ty: 2,
          nm: "Layer 1",
          refId: "image_1",
          sr: 1,
          ks: {
            o: { a: 0, k: 100, ix: 11 },
            r: { a: 0, k: 0, ix: 10 },
            p: { a: 0, k: [960, 540, 0], ix: 2 },
            a: { a: 0, k: [301.921, 158.427, 0], ix: 1 },
            s: { a: 0, k: [100, 100, 100], ix: 6 }
          },
          ao: 0,
          ip: 0,
          op: 180.00000733155,
          st: 0,
          bm: 0
        }
      ]
    }
  ],
  layers: [
    {
      ddd: 0,
      ind: 1,
      ty: 0,
      nm: "Digital Documents and Data",
      refId: "comp_0",
      sr: 1,
      ks: {
        o: { a: 0, k: 100, ix: 11 },
        r: { a: 0, k: 0, ix: 10 },
        p: { a: 0, k: [296, 482, 0], ix: 2 },
        a: { a: 0, k: [960, 540, 0], ix: 1 },
        s: { a: 0, k: [100, 100, 100], ix: 6 }
      },
      ao: 0,
      w: 1920,
      h: 1080,
      ip: 0,
      op: 180.00000733155,
      st: 0,
      bm: 0
    }
  ],
  markers: []
};
```

3. Use it like the following:

CustomSoftware.js
```jsx
import React from 'react'
import Lottie from 'react-lottie'
import {Link} from 'react-router-dom'
import {makeStyles, useTheme} from '@material-ui/core/styles'
import Grid from '@material-ui/core/Grid'
import IconButton from '@material-ui/core/IconButton'
import Typography from '@material-ui/core/Typography'
import useMediaQuery from '@material-ui/core/useMediaQuery'
import Hidden from '@material-ui/core/Hidden'

import backArrow from '../assets/backArrow.svg'
import forwardArrow from '../assets/forwardArrow.svg'
import lightbulb from '../assets/bulb.svg'
import cash from '../assets/cash.svg'
import stopwatch from '../assets/stopwatch.svg'
import roots from '../assets/root.svg'
import documentsAnimation from '../animations/documentsAnimation/data'
import scaleAnimation from '../animations/scaleAnimation/data'
import automationAnimation from '../animations/automationAnimation/data'
import uxAutomation from '../animations/uxAnimation/data'

import CallToAction from './ui/CallToAction'

const useStyles = makeStyles(theme => {
  return {
    heading: {
      maxWidth: '40em'
    },
    arrowContainer: {
      marginTop: '0.5em'
    },
    rowContainer: {
      paddingLeft: '5em',
      paddingRight: '5em',
      [theme.breakpoints.down('sm')]: {
        paddingLeft: '1.5em',
        paddingRight: '1.5em'
      }
    },
    itemContainer: {
      maxWidth: '40em'
    }
  }
})

const documentsOptions = {
  loop: true,
  autoplay: false,
  animationData: documentsAnimation,
  rendererSettings: {
    preserveAspectRatio: 'xMidYMid slice'
  }
};

const scaleOptions = {
  loop: true,
  autoplay: false,
  animationData: scaleAnimation,
  rendererSettings: {
    preserveAspectRatio: 'xMidYMid slice'
  }
};

const automationOptions = {
  loop: true,
  autoplay: false,
  animationData: automationAnimation,
  rendererSettings: {
    preserveAspectRatio: 'xMidYMid slice'
  }
};

const uxOptions = {
  loop: true,
  autoplay: false,
  animationData: uxAutomation,
  rendererSettings: {
    preserveAspectRatio: 'xMidYMid slice'
  }
};

export default function CustomSoftware(props) {
  const classes = useStyles()
  const theme = useTheme()
  const matchedMD = useMediaQuery(theme.breakpoints.down('md'))
  const matchesSM = useMediaQuery(theme.breakpoints.down('sm'))
  const matchesXS = useMediaQuery(theme.breakpoints.down('xs'))

  return <Grid container direction={'column'}>

    <Grid item container className={classes.rowContainer} direction={'row'} justify={matchedMD ? 'center' : undefined} style={{marginTop: matchesXS ? '1em' : '2em'}}>
      {/**CARDS WITH CONTROLS**/}

      <Hidden mdDown>
        <Grid
          item
          className={classes.arrowContainer}
          style={{marginRight: '1em', marginLeft: '-3.5em'}}
        >
          <IconButton
            style={{backgroundColor: 'transparent'}}
            component={Link}
            to={'/services'}
            onClick={() => {props.setSelectedIndex()}}
          >
            <img src={backArrow} alt={'Back to services page'}/>
          </IconButton>
        </Grid>
      </Hidden>

      <Grid item container direction={'column'} className={classes.heading}>
        <Grid item>
          <Typography
            variant={'h2'}
            align={matchedMD ? 'center' : undefined}
          >
            Custom software development
          </Typography>
          <Typography
            variant={'body1'}
            paragraph
            align={matchedMD ? 'center' : undefined}
          >
            Whether we're replacing old software or inventing new solutions,
            Arc Development is here to help your business tackle technology.
          </Typography>
          <Typography
            variant={'body1'}
            paragraph
            align={matchedMD ? 'center' : undefined}
          >
            Using regular commercial software leaves you with a lot of stuff you don't need, without the stuff you need, and ultimately controls the way you work.
            Without using any software at all you risk falling behind competitors and missing out on huge savings from increased efficiency.
          </Typography>
          <Typography
            variant={'body1'}
            paragraph
            align={matchedMD ? 'center' : undefined}
          >
            Our custom solutions are designed from the ground up with your need, wants and goals at the core. this collaborative process produces finely tuned software
            that is much more effective at improving your workflow and reducing costs than generalized options.
          </Typography>
          <Typography
            variant={'body1'}
            paragraph
            align={matchedMD ? 'center' : undefined}
          >
            We create exactly what you want, exactly how you want it.
          </Typography>
        </Grid>
      </Grid>

      <Hidden mdDown>
        <Grid item className={classes.arrowContainer}>
          <IconButton
            style={{backgroundColor: 'transparent'}}
            component={Link}
            to={'/mobileapps'}
            onClick={() => props.setSelectedIndex(2)}
          >
            <img src={forwardArrow} alt={'Forward to iOS/Android App Development Page'}/>
          </IconButton>
        </Grid>
      </Hidden>

    </Grid>

    <Grid item container className={classes.rowContainer} direction={'row'} justify={'center'} style={{marginTop: '15em', marginBottom: '15em'}}>
      <Grid item container direction={'column'} md  alignItems={'center'} style={{maxWidth: '40em'}}>
        <Grid item>
          <Typography variant={'h4'}>
            Save Energy
          </Typography>
        </Grid>
        <Grid item>
          <img src={lightbulb} alt={'lightbulb'}/>
        </Grid>
      </Grid>
      <Grid item container direction={'column'} md alignItems={'center'} style={{maxWidth: '40em', marginBottom: matchesSM ? '10em' : 0, marginTop: matchesSM ? '10em' : 0}}>
        <Grid item>
          <Typography variant={'h4'}>
            Save Time
          </Typography>
        </Grid>
        <Grid item>
          <img src={stopwatch} alt={'stopwatch'}/>
        </Grid>
      </Grid>
      <Grid item container direction={'column'} md  alignItems={'center'} style={{maxWidth: '40em'}}>
        <Grid item>
          <Typography variant={'h4'}>
            Save Cash
          </Typography>
        </Grid>
        <Grid item>
          <img src={cash} alt={'cash'}/>
        </Grid>
      </Grid>
    </Grid>

    <Grid
      item
      container
      direction={matchedMD ? 'column' : 'row'}
      alignItems={matchedMD ? 'center' : undefined}
      justify={'space-around'}
      style={{marginBottom: '10em'}}
      className={classes.rowContainer}
    >
      <Grid item container direction={matchesSM ? 'column' : 'row'} className={classes.itemContainer} md style={{marginBottom: matchedMD ? '15em' : 0}}>

        <Grid item container direction={'column'} md>
          <Grid item>
            <Typography variant={'h4'} align={matchesSM ? 'center' : undefined}>Digital documents & data</Typography>
          </Grid>
          <Grid item>
            <Typography variant={'body1'} paragraph align={matchesSM ? 'center' : undefined}>
              Reduce Errors. Reduce Waste. Reduce Costs.
            </Typography>
            <Typography variant={'body1'} paragraph align={matchesSM ? 'center' : undefined}>
              Billions are spent annually on the purchasing, printing, and distribution of paper. On top of the massive environmental impact this has, it causes harm to your bottom line as well.
            </Typography>
            <Typography variant={'body1'} paragraph align={matchesSM ? 'center' : undefined}>
              By utilizing digital forms and documents you can remove these obsolete expenses, accelerate your communication, and help the Earth.
            </Typography>
          </Grid>
        </Grid>

        <Grid item md>
          <Lottie options={documentsOptions} isStopped={true} style={{maxHeight: 265, maxWidth: 275, minHeight: 250}}/>
        </Grid>

      </Grid>
      <Grid item container direction={matchesSM ? 'column' : 'row'} className={classes.itemContainer} md>

        <Grid item md>
          <Lottie options={scaleOptions} isStopped={true} style={{maxHeight: 325, maxWidth: 280, minHeight: 260}}/>
        </Grid>

        <Grid item container direction={'column'} md>
          <Grid item>
            <Typography variant={'h4'} align={matchesSM ? 'center' : 'right'}>Scale</Typography>
          </Grid>
          <Grid item>
            <Typography variant={'body1'} align={matchesSM ? 'center' : 'right'} paragraph>
              Whether you’re a large brand, just getting started, or taking off right now, our application architecture ensures pain-free growth and reliability.
            </Typography>
          </Grid>
        </Grid>

      </Grid>
    </Grid>

    <Grid item container direction={'row'} className={classes.rowContainer} style={{marginTop: '20em', marginBottom: '20em'}}>
      {/** TREE **/}
      <Grid item container direction={'column'} alignItems={'center'}>
        <Grid item>
          <img src={roots} alt={'tree with roots extending out'} height={matchesSM ? '300em' : '450em'} width={matchesSM ? '300em' : '450em'}/>
        </Grid>
        <Grid item className={classes.itemContainer}>
          <Typography variant={'h4'} align={'center'} gutterBottom>Root-cause analysis</Typography>
          <Typography variant={'body1'} align={'center'} paragraph>Many problems are merely symptoms of larger, underlying issues.</Typography>
          <Typography variant={'body1'} align={'center'} paragraph>We can help you thoroughly examine all areas of your business to develop a holistic plan for the most effective implementation of technology.</Typography>
        </Grid>
      </Grid>
    </Grid>

    <Grid item
          container
          className={classes.rowContainer}
          justify={'space-around'}
          style={{marginBottom: '20em'}}
          direction={matchedMD ? 'column' : 'row'}
          alignItems={matchedMD ? 'center' : undefined}>
      <Grid item container direction={matchesSM ? 'column' : 'row'} className={classes.itemContainer} style={{marginBottom: matchedMD ? '15em' : 0}} md>

        <Grid item container direction={'column'} md>
          <Grid item>
            <Typography variant={'h4'} align={matchesSM ? 'center' : undefined}>Automation</Typography>
          </Grid>
          <Grid item>
            <Typography variant={'body1'} paragraph align={matchesSM ? 'center' : undefined}>
              Why waste time when you don’t have to?
            </Typography>
            <Typography variant={'body1'} paragraph align={matchesSM ? 'center' : undefined}>
              We can help you identify processes with time or event based actions which can now easily be automated.
            </Typography>
            <Typography variant={'body1'} paragraph align={matchesSM ? 'center' : undefined}>
              Increasing efficiency increases profits, leaving you more time to focus on your business, not busywork.
            </Typography>
          </Grid>
        </Grid>

        <Grid item md>
          <Lottie options={automationOptions} isStopped={true} style={{maxHeight: 290, maxWidth: 280}}/>
        </Grid>

      </Grid>
      <Grid item container direction={matchesSM ? 'column' : 'row'} className={classes.itemContainer} md>

        <Grid item md>
          <Lottie options={uxOptions} isStopped={true} style={{maxHeight: 310, maxWidth: 155}}/>
        </Grid>

        <Grid item container direction={'column'} md>
          <Grid item>
            <Typography variant={'h4'} align={matchesSM ? 'center' : 'right'}>User Experience Design</Typography>
          </Grid>
          <Grid item>
            <Typography variant={'body1'} align={matchesSM ? 'center' : 'right'} paragraph>
              A good design that isn’t usable isn’t a good design.
            </Typography>
            <Typography variant={'body1'} align={matchesSM ? 'center' : 'right'} paragraph>
              So why are so many pieces of software complicated, confusing, and frustrating?
            </Typography>
            <Typography variant={'body1'} align={matchesSM ? 'center' : 'right'} paragraph>
              By prioritizing users and the real ways they interact with technology we’re able to develop unique, personable experiences that solve problems rather than create new ones.
            </Typography>
          </Grid>
        </Grid>

      </Grid>
    </Grid>


    <Grid item>
      <CallToAction setValue={props.setValue}/>
    </Grid>
  </Grid>
}
```
