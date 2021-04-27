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

Survey


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
