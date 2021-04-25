---
title: React
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
import React from 'react'
import PropTypes from 'prop-types'
import Todo from './Todo'

const TodoList = ({todos, onTodoClick}) => {
  return <ul>
    {todos.map((todo, index) => {
      return <Todo key={todo.id} {...todo} onClick={() => onTodoClick(todo)}/>
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
import React from 'react'
import PropTypes from 'prop-types'

const Todo = ({onClick, completed, text}) => {
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
import React from 'react'

function logHoc (WrappedComponent) {
    return class extends React.Component {
      render(){
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
import React, {Component} from 'react';
import {connect} from 'react-redux'
import {Link} from "react-router-dom";
import Payments from "./Payments";

class Header extends Component {
  renderContent() {
    switch (this.props.auth) {
      case null:
        return
      case false:
        return (
          <li>
            <a href="/auth/google" >Login with google</a>
          </li>
        )
      default:
        return [
          <li key="1"><Payments/></li>,
          <li key="3" style={{ margin: '0 10px'}}>
            Credits: {this.props.auth.credits}
          </li>,
          <li key="2"><a href='/api/logout'>Logout</a></li>
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

function mapStateToProps (state) {
  return {
    auth: state.auth
  }
}

export default connect(mapStateToProps, null)(Header);
```

<br />

Link.js
```javascript
import React from 'react'
import {connect} from 'react-redux'
import {setVisibilityFilter} from '../actions'
import Link from '../components/Link'

const mapStateToProps = (state, ownProps) => {
  return {active: state.visibilityFilter === ownProps.filter}
}

const mapDispatchToProps = (dispatch, ownProps) => {
  return {
    onClick: () => {dispatch(setVisibilityFilter(ownProps.filter))}
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(Link)
```

<br />

## Libraries

### Stripe.js 
Dependency used `"react-stripe-checkout": "^2.6.3",`. Following is an example about how to use
the library.

Payments.js

```jsx
import React, {Component} from 'react';
import StripeCheckout from 'react-stripe-checkout'
import {connect} from 'react-redux'
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
import React, {Component} from 'react';
import SurveyForm from "./SurveyForm";
import SurveyFormReview from './SurveyFormReview'
import {reduxForm} from "redux-form";

class SurveyNew extends Component {

  state = {
    showFormReview: false
  }

  renderContent() {
    if (this.state.showFormReview === true) {
      return <SurveyFormReview
        onCancel={() => this.setState( { showFormReview: false })}
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
import React, {Component} from 'react';
import {reduxForm, Field} from "redux-form";
import SurveyField from "./SurveyField";
import _ from 'lodash'
import {Link} from 'react-router-dom'
import validateEmails from "../../utils/validateEmails";
import formFields from './formFields'

class SurveyForm extends Component {

  renderFields() {
    return (
      _.map(formFields, ({label, name}) => {
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

  _.each(formFields, ({name}) => {
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
import React from 'react';

const SurveyField = ({ input, label, meta: {error, touched} }) => {
  return (
    <div>
      <label>{label}</label>
      <input {...input} style={{ marginBottom: '5px'}} />
      <div className="red-text" style={{ marginBottom: '20px'}}>
        { touched && error }
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
import React from 'react';
import {connect} from 'react-redux'
import formFields from './formFields'
import _ from 'lodash'
import {withRouter} from 'react-router-dom'
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

function mapStateToProps (state) {
  return {
    formValues: state.form.surveyForm.values
  }
}

export default connect(mapStateToProps, actions)(withRouter(SurveyFormReview));
```
