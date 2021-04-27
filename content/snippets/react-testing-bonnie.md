---
title: React testing (Bonnie Shulkin)
---

## Introduction

setupTests.js
```javascript
import Enzyme, {shallow} from 'enzyme'
import EnzymeAdapter from 'enzyme-adapter-react-16'

Enzyme.configure({
  adapter: new EnzymeAdapter(),
  disableLifecycleMethods: true
})
```

<br />

utils.js
```javascript
import checkPropTypes from 'check-prop-types'
import {createStore, applyMiddleware} from 'redux'

import rootReducer from '../src/reducers'
import {middlewares} from '../src/configureStore'

export const storeFactory = (initialState) => {
  const createStoreWithMiddleware = applyMiddleware(...middlewares)(createStore)

  return createStoreWithMiddleware(rootReducer, initialState)
}

export const findByTestAttr = (wrapper, val) => {
  return wrapper.find(`[data-test="${val}"]`)
}

export const checkProps = (component, conformingProps) => {
  const propError = checkPropTypes(
    component.propTypes,
    conformingProps,
    'prop',
    component.name
  )
  expect(propError).toBeUndefined()
}
```

<br />

package.json
```json
{
  "name": "jotto",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@babel/core": "7.12.3",
    "@pmmmwh/react-refresh-webpack-plugin": "0.4.2",
    "@svgr/webpack": "5.4.0",
    "@testing-library/jest-dom": "^5.11.5",
    "@testing-library/react": "^11.1.2",
    "@testing-library/user-event": "^12.2.2",
    "@typescript-eslint/eslint-plugin": "^4.5.0",
    "@typescript-eslint/parser": "^4.5.0",
    "axios": "^0.21.0",
    "babel-eslint": "^10.1.0",
    "babel-jest": "^26.6.0",
    "babel-loader": "8.1.0",
    "babel-plugin-named-asset-import": "^0.3.7",
    "babel-preset-react-app": "^10.0.0",
    "bfj": "^7.0.2",
    "camelcase": "^6.1.0",
    "case-sensitive-paths-webpack-plugin": "2.3.0",
    "css-loader": "4.3.0",
    "dotenv": "8.2.0",
    "dotenv-expand": "5.1.0",
    "eslint": "^7.11.0",
    "eslint-config-react-app": "^6.0.0",
    "eslint-plugin-flowtype": "^5.2.0",
    "eslint-plugin-import": "^2.22.1",
    "eslint-plugin-jest": "^24.1.0",
    "eslint-plugin-jsx-a11y": "^6.3.1",
    "eslint-plugin-react": "^7.21.5",
    "eslint-plugin-react-hooks": "^4.2.0",
    "eslint-plugin-testing-library": "^3.9.2",
    "eslint-webpack-plugin": "^2.1.0",
    "file-loader": "6.1.1",
    "fs-extra": "^9.0.1",
    "html-webpack-plugin": "4.5.0",
    "identity-obj-proxy": "3.0.0",
    "jest": "26.6.0",
    "jest-circus": "26.6.0",
    "jest-resolve": "26.6.0",
    "jest-watch-typeahead": "0.6.1",
    "mini-css-extract-plugin": "0.11.3",
    "optimize-css-assets-webpack-plugin": "5.0.4",
    "pnp-webpack-plugin": "1.6.4",
    "postcss-flexbugs-fixes": "4.2.1",
    "postcss-loader": "3.0.0",
    "postcss-normalize": "8.0.1",
    "postcss-preset-env": "6.7.0",
    "postcss-safe-parser": "5.0.2",
    "prop-types": "^15.7.2",
    "react": "^16.14.0",
    "react-app-polyfill": "^2.0.0",
    "react-dev-utils": "^11.0.0",
    "react-dom": "^16.14.0",
    "react-redux": "^7.2.2",
    "react-refresh": "^0.8.3",
    "redux": "^4.0.5",
    "redux-thunk": "^2.3.0",
    "resolve": "1.18.1",
    "resolve-url-loader": "^3.1.2",
    "sass-loader": "8.0.2",
    "semver": "7.3.2",
    "style-loader": "1.3.0",
    "terser-webpack-plugin": "4.2.3",
    "ts-pnp": "1.2.0",
    "url-loader": "4.1.1",
    "web-vitals": "^0.2.4",
    "webpack": "4.44.2",
    "webpack-dev-server": "3.11.0",
    "webpack-manifest-plugin": "2.2.0",
    "workbox-webpack-plugin": "5.1.4"
  },
  "scripts": {
    "start": "node scripts/start.js",
    "build": "node scripts/build.js",
    "test": "node scripts/test.js"
  },
  "eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest"
    ]
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  },
  "devDependencies": {
    "babel-plugin-react-remove-properties": "^0.3.0",
    "check-prop-types": "^1.1.2",
    "enzyme": "^3.11.0",
    "enzyme-adapter-react-16": "^1.15.5",
    "jest-enzyme": "^7.1.2",
    "moxios": "^0.4.0"
  },
  "jest": {
    "roots": [
      "<rootDir>/src"
    ],
    "collectCoverageFrom": [
      "src/**/*.{js,jsx,ts,tsx}",
      "!src/**/*.d.ts"
    ],
    "setupFiles": [
      "react-app-polyfill/jsdom"
    ],
    "setupFilesAfterEnv": [
      "<rootDir>/src/setupTests.js"
    ],
    "testMatch": [
      "<rootDir>/src/**/__tests__/**/*.{js,jsx,ts,tsx}",
      "<rootDir>/src/**/*.{spec,test}.{js,jsx,ts,tsx}"
    ],
    "testEnvironment": "jsdom",
    "testRunner": "D:\\Projects\\Javascript\\jotto\\node_modules\\jest-circus\\runner.js",
    "transform": {
      "^.+\\.(js|jsx|mjs|cjs|ts|tsx)$": "<rootDir>/node_modules/babel-jest",
      "^.+\\.css$": "<rootDir>/config/jest/cssTransform.js",
      "^(?!.*\\.(js|jsx|mjs|cjs|ts|tsx|css|json)$)": "<rootDir>/config/jest/fileTransform.js"
    },
    "transformIgnorePatterns": [
      "[/\\\\]node_modules[/\\\\].+\\.(js|jsx|mjs|cjs|ts|tsx)$",
      "^.+\\.module\\.(css|sass|scss)$"
    ],
    "modulePaths": [],
    "moduleNameMapper": {
      "^react-native$": "react-native-web",
      "^.+\\.module\\.(css|sass|scss)$": "identity-obj-proxy"
    },
    "moduleFileExtensions": [
      "web.js",
      "js",
      "web.ts",
      "ts",
      "web.tsx",
      "tsx",
      "json",
      "web.jsx",
      "jsx",
      "node"
    ],
    "watchPlugins": [
      "jest-watch-typeahead/filename",
      "jest-watch-typeahead/testname"
    ],
    "resetMocks": true
  },
  "babel": {
    "env": {
      "production": {
        "plugins": [
          [
            "react-remove-properties",
            {
              "properties": [
                "data-test"
              ]
            }
          ]
        ]
      }
    },
    "presets": [
      "react-app"
    ]
  }
}
```

<br />

## Component tests

### Input

Input.js
```jsx
import React, {Component} from 'react'

import {connect} from 'react-redux'
import {guessWord, reset} from './actions'

export class UnconnectedInput extends Component {
  constructor(props) {
    super(props);

    //initialize the state
    this.state = {
      currentGuess: ''
    }

    this.handleOnSubmit = this.handleOnSubmit.bind(this)
  }

  handleOnSubmit (e) {
    e.preventDefault()
    const guessedWord = this.state.currentGuess
    if (guessedWord && guessedWord.length > 0) {
      this.props.guessWord(this.state.currentGuess)
      this.setState({currentGuess: ''})
    }
  }

  render() {

    const contents = !this.props.success
      ? <form>
          <input
            data-test="input-box"
            className="mb-2 mx-sm-3"
            value={this.state.currentGuess}
            onChange={(e) => {
              this.setState({currentGuess: e.target.value})
            }}
          />
          <button
            data-test="submit-button"
            type="submit"
            className="btn btn-primary mb-2"
            onClick={(e) => {this.handleOnSubmit(e)}}
          >
            Submit
          </button>
        </form>
      : <form>
          <button
            data-test="reset-button"
            className="btn btn-secondary ml-2"
          >
            Reset
          </button>
      </form>

    return <div data-test="component-input">
      {contents}
    </div>
  }
}

const mapStateToProps = ({success}) => {
  return {
    success
  }
}

const mapDispatchToProps = (dispatch, ownProps) => {
  return {
    guessWord: guessWord,
    reset: reset
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(UnconnectedInput)
```

<br />

Input.test.js
```javascript
import React from 'react'
import {shallow} from 'enzyme'

import {findByTestAttr, storeFactory} from '../test/testUtils'
import Input, {UnconnectedInput} from './Input'
import {guessWord} from './actions'

const setup = (initialState = {}) => {
  const store = storeFactory(initialState)
  return shallow(<Input store={store} />).dive().dive()
}

describe('render', () => {
  describe('word has not been guessed', () => {
    let wrapper
    beforeEach(() => {
      const initialState = {success: false}
      wrapper = setup(initialState)
    })
    test('renders component without error', () => {
      const component = findByTestAttr(wrapper, 'component-input')
      expect(component.length).toBe(1)
    })
    test('renders input box', () => {
      const inputBox = findByTestAttr(wrapper, 'input-box')
      expect(inputBox.length).toBe(1)
    })
    test('renders submit button', () => {
      const submitButton = findByTestAttr(wrapper, 'submit-button')
      expect(submitButton.length).toBe(1)
    })
    test('does not render reset button', () => {
      const submitButton = findByTestAttr(wrapper, 'reset-button')
      expect(submitButton.length).toBe(0)
    })
  })
  describe('word has been guessed', function () {
    let wrapper
    beforeEach(() => {
      wrapper = setup({success: true})
    })
    test('renders component without error', () => {
      const component = findByTestAttr(wrapper, 'component-input')
      expect(component.length).toBe(1)
    })
    test('does not render input box', () => {
      const inputBox = findByTestAttr(wrapper, 'input-box')
      expect(inputBox.length).toBe(0)
    })
    test('does not render submit button', () => {
      const submitButton = findByTestAttr(wrapper, 'submit-button')
      expect(submitButton.length).toBe(0)
    })
    test('render reset button', () => {
      const submitButton = findByTestAttr(wrapper, 'reset-button')
      expect(submitButton.length).toBe(1)
    })
  });
})

describe('redux props', () => {
  test('has success piece of state as prop', () => {
    const success = true
    const wrapper = setup({success})
    const successProp = wrapper.instance().props.success
    expect(successProp).toBe(success)
  })
  test('`guessWord` action creator is a function prop', () => {
    const success = true
    const wrapper = setup({success})
    const guessWordProp = wrapper.instance().props.guessWord
    expect(guessWordProp).toBeInstanceOf(Function)
  })
})

describe('`guessWord` action creator call', () => {
  let guessWordMock;
  let wrapper;
  const guessedWord = 'train'

  beforeEach(() => {
    // create a mock function for `getSecretWord`
    guessWordMock = jest.fn()

    const props = {guessWord: guessWordMock, success: false}

    wrapper = shallow(<UnconnectedInput {...props} />)

    //add value to input box
    wrapper.setState({currentGuess: guessedWord})

    const button = findByTestAttr(wrapper, 'submit-button')

    //simulate click on submit button
    button.simulate('click', {preventDefault() {}})
  })


  test('calls `guessWord` when button is clicked', () => {
    const guessWordMockCount = guessWordMock.mock.calls.length

    expect(guessWordMockCount).toBe(1)
  })

  test('calls `guessWord with input value as argument`', () => {
    const guessWordArg = guessWordMock.mock.calls[0][0]
    expect(guessWordArg).toBe(guessedWord)
  })

  test('clear input box on submit', () => {
    expect(wrapper.state('currentGuess')).toBe('')
  })
})
```

<br />

### Normal component with props #1

GuessedWords.js

```jsx
import React from 'react'
import PropTypes from 'prop-types'

const GuessedWords = (props) => {
  let contents
  if (props.guessedWords.length === 0) {
    contents = (
      <span data-test="guess-instructions">
        Try to guess the secret word!
      </span>
    )
  } else {
    const guessedWordsRows = props.guessedWords.map((word, index) => {
      return (
        <tr key={index} data-test="guessed-word">
          <td>
            {word.guessedWord}
          </td>
          <td>
            {word.letterMatchCount}
          </td>
        </tr>
      )
    })
    contents = (
      <div data-test="guessed-words">
        <h3>Guessed words</h3>
        <table className="table table-sm">
          <thead className="thead-light">
            <tr>
              <th>
                Guess
              </th>
              <th>
                Matching Letters
              </th>
            </tr>
          </thead>
          <tbody>
            {guessedWordsRows}
          </tbody>
        </table>
      </div>
    )
  }
  return (
    <div data-test="component-guessed-words">
      {contents}
    </div>
  )
}

GuessedWords.propTypes = {
  guessedWords: PropTypes.arrayOf(
    PropTypes.shape({
      guessedWord: PropTypes.string.isRequired,
      letterMatchCount: PropTypes.number.isRequired
    })
  ).isRequired
}

export default GuessedWords
```

<br />


GuessedWords.test.js
```javascript
import React from 'react'
import {shallow} from 'enzyme'
import {findByTestAttr, checkProps} from '../test/testUtils'
import GuessedWords from './GuessedWords'

const defaultProps = {
    guessedWords: [{guessedWord: 'train', letterMatchCount: 3}]
  }


const setUp = (props = {}) => {
  const setUpProps = {...defaultProps, ...props}
  return shallow(<GuessedWords {...setUpProps} />)
}

test('does not throw warning with expected props', () => {
  checkProps(GuessedWords, defaultProps)
})

describe('if there are no words guessed', () => {

  let wrapper
  beforeEach(() => {
    wrapper = setUp({guessedWords: []})
  })

  test('renders without error', () => {
    const component = findByTestAttr(wrapper, 'component-guessed-words')
    expect(component.length).toBe(1)
  })
  test('renders instruction to guess a word', () => {
    const instructions = findByTestAttr(wrapper, 'guess-instructions')
    expect(instructions.text().length).not.toBe(1)
  })
})

describe('if there are words guessed', () => {
  const guessedWords = [
    {guessedWord: 'train', letterMatchCount: 3},
    {guessedWord: 'agile', letterMatchCount: 1},
    {guessedWord: 'party', letterMatchCount: 5}
  ]
  let wrapper
  beforeEach(() => {
    wrapper = setUp({guessedWords})
  })
  test('renders without error', () => {
    const component = findByTestAttr(wrapper, 'component-guessed-words')
    expect(component.length).toBe(1)
  })
  test('renders "guessed words" section', () => {
    const guessedWordsNode = findByTestAttr(wrapper, 'guessed-words')
    expect(guessedWordsNode.length).toBe(1)
  })
  test('correct number of guessed words', () => {
    const guessedWordsNodes = findByTestAttr(wrapper, 'guessed-word')
    expect(guessedWordsNodes.length).toBe(guessedWords.length)
  })
})
```

<br />

### Normal component with props #2

GuessedWordsMessage.js

```jsx
import React from 'react'
import PropTypes from 'prop-types'

class GuessedWordMessage extends React.Component {
  render() {

    const message = this.props.guessedWords.length > 0
      ? <div>
          <p data-test="guess-word-message">
            Words guessed {this.props.guessedWords.length}
          </p>
        </div>
      : null

    return (
      message
    )
  }
}

GuessedWordMessage.propTypes = {
  guessedWords: PropTypes.arrayOf(
    PropTypes.shape({
      guessedWord: PropTypes.string.isRequired,
      letterMatchCount: PropTypes.number.isRequired
    })
  ).isRequired
}


export default GuessedWordMessage
```

<br />

GuessedWordsMessage.test.js
```javascript
import React from 'react'
import {shallow} from 'enzyme'
import GuessedWordMessage from './GuessedWordMessage'
import {findByTestAttr} from '../test/testUtils'

const defaultProps = {guessedWords: []}


const setUp = (props = {}) => {
  const setUpProps = {...defaultProps, ...props}
  const wrapper = shallow(<GuessedWordMessage {...setUpProps} />)
  wrapper.debug()
  return wrapper
}

setUp()

describe('if there are no words guessed', () => {
  let wrapper

  beforeEach(() => {
    wrapper = setUp()
  })

  test('does not render', () => {
    const component = findByTestAttr(wrapper,'guess-word-message')
    expect(component.length).toBe(0)
  })
})

describe('if there are words guessed', () => {
  let wrapper

  beforeEach(() => {
    wrapper = setUp({guessedWords: [{guessedWord: 'bycycle', letterMatchCount: 2}]})
  })

  test('does not render', () => {
    const component = findByTestAttr(wrapper,'guess-word-message')
    expect(component.length).toBe(1)
  })
})
```

<br />

### Normal component with props #3

Congrats.js
```javascript
import React from 'react'
import PropTypes from 'prop-types'

/**
 * @function
 * @param {object} props
 * @returns {JSX.element}
 */
const Congrats = (props) => {
  if (props.success) {
    return (
      <div data-test="component-congrats" className="alert alert-success">
        <span data-test="congrats-message">
          Congratulations! you guessed the word!
        </span>
      </div>
    )
  } else {
    return (
      <div data-test="component-congrats">

      </div>
    )
  }
}

Congrats.propTypes = {
  success: PropTypes.bool.isRequired
}

export default  Congrats
```

<br />

Congrats.test.js
```javascript
import React from 'react'
import {shallow} from 'enzyme'
import {checkProps, findByTestAttr} from '../test/testUtils'
import Congrats from './Congrats'

const defaultProps = {success: false}

/**
 * @function
 * @param {object}props
 * @returns {ShallowWrapper}
 */
const setup = (props = {}) => {
  const setUpProps = {...defaultProps, ...props}
  return shallow(<Congrats {...setUpProps} />)
}

test('render without error', () => {
  const wrapper = setup({success: false})
  const component = findByTestAttr(wrapper, 'component-congrats')
  expect(component.length).toBe(1)
})

test('renders no text when success prop is false', () => {
  const wrapper = setup({success: false})
  const component = findByTestAttr(wrapper, 'component-congrats')
  expect(component.text()).toBe('')

})

test('renders non-empty congrats message when succe', () => {
  const wrapper = setup({success: true})
  const message = findByTestAttr(wrapper, 'congrats-message')
  expect(message.text().length).not.toBe(0)
})

test('does not throw a warning with expected props', () => {
  const expectedProps = {success: false}
  const propError = checkProps(Congrats, expectedProps)
})
```

<br />

### Normal component with props #4

App.js

```javascript
import React, {Component} from 'react'
import logo from './logo.svg';
import './App.css';
import {connect} from 'react-redux'
import GuessedWords from './GuessedWords'
import Congrats from './Congrats'
import {getSecretWord} from './actions'
import Input from './Input'

export class UnconnectedApp extends Component {

  componentDidMount() {
    //get the secret word
    this.props.getSecretWord()
  }

  render() {
    return (
      <div className="container">
        <h1>Jotto</h1>
        <Congrats success={this.props.success} />
        <Input />
        <GuessedWords guessedWords={this.props.guessedWords}/>
      </div>
    );
  }
}

const mapStateToProps = (state) => {
  const {success, guessedWords, secretWord} = state
  return {
    success,
    guessedWords,
    secretWord
  }
}

const mapDispatchToProps = (dispatch, state) => {
  return {
    getSecretWord
  }
}


export default connect(mapStateToProps, mapDispatchToProps)(UnconnectedApp);

```

<br />

App.test.js
```javascript
import React from 'react'
import {storeFactory} from '../test/testUtils'
import {shallow} from 'enzyme'
import App, {UnconnectedApp} from './App'

const setup = (initialState = {}) => {
  const store = storeFactory(initialState)
  return shallow(<App store={store}/>).dive().dive()
}

describe('redux props', () => {
  test('has access to `success` state', () => {
    const success = true
    const wrapper = setup({success})
    const successProp = wrapper.instance().props.success
    expect(successProp).toBe(success)
  })
  test('has access to `secret word` state', () => {
    const secretWord = 'party'
    const wrapper = setup({secretWord})
    const secretWordProp = wrapper.instance().props.secretWord
    expect(secretWordProp).toBe(secretWord)
  })
  test('has access to `guessedWords` state', () => {
    const guessedWords = [{guessedWord: 'train', letterMatchCount: 3}]
    const wrapper = setup({guessedWords})
    const guessedWordsProp = wrapper.instance().props.guessedWords
    expect(guessedWords).toEqual(guessedWordsProp)
  })
  test('`getSecretWord` action creator is a function on the props', () => {
    const wrapper = setup()
    const getSecretWordProp = wrapper.instance().props.getSecretWord
    expect(getSecretWordProp).toBeInstanceOf(Function)
  })
})

test('`getSecretWord` runs on app mount', () => {
  const getSecretWordMock = jest.fn()

  const props = {success: false, getSecretWord: getSecretWordMock, guessedWords: []}

  //set up app component with getSecretWordMock as the getSecretWord prop
  const wrapper = shallow(<UnconnectedApp {...props} />)

  //run lifecycle method
  wrapper.instance().componentDidMount()

  //check to see if mock ran
  const getSecretWordCallCount = getSecretWordMock.mock.calls.length

  expect(getSecretWordCallCount).toBe(1)
});

```

<br />

## Integration tests

integration.test.js
```javascript
import {storeFactory} from '../test/testUtils'
import {guessWord, reset} from './actions'

describe('guessWord action dispatcher', () => {

  const secretWord = 'party'
  const unsuccessfulGuess = 'train'

  describe('no guessed words', () => {

    let store
    const initialState = {secretWord}

    beforeEach(() => {
      store = storeFactory(initialState)
    })

    test('updates state correctly for unsuccessful guess', () => {
      store.dispatch(guessWord(unsuccessfulGuess))
      const newState = store.getState()
      const expectedState = {
        ...initialState,
        success: false,
        serverError: false,
        guessedWords: [{
          guessedWord: unsuccessfulGuess,
          letterMatchCount: 3
        }]
      }
      expect(newState).toEqual(expectedState)
    })

    test('updates state correctly for successful guess', () => {
      store.dispatch(guessWord(secretWord))
      const newState = store.getState()
      const expectedState = {
        secretWord,
        success: true,
        serverError: false,
        guessedWords: [{
          guessedWord: secretWord,
          letterMatchCount: 5
        }]
      }
      expect(newState).toEqual(expectedState)
    })

  })
  describe('some guessed words', () => {
    const guessedWords = [ { guessedWord: 'agile', letterMatchCount: 1} ]
    const initialState = {guessedWords, secretWord}
    let store
    beforeEach(() => {
      store = storeFactory(initialState)
    })
    test('updates state correctly for unsuccessful guess', () => {
      store.dispatch(guessWord(unsuccessfulGuess))
      const newState = store.getState()
      const expectedState = {
        secretWord,
        success: false,
        serverError: false,
        guessedWords: [
          ...guessedWords,
          { guessedWord: unsuccessfulGuess, letterMatchCount: 3}
        ]
      }
      expect(newState).toEqual(expectedState)
    })
    test('updates state correctly for successful guess', () => {
      store.dispatch(guessWord(secretWord))
      const newState = store.getState()
      const expectedState = {
        secretWord,
        success: true,
        serverError: false,
        guessedWords: [
          ...guessedWords,
          {guessedWord: secretWord, letterMatchCount: 5}
        ]
      }
      expect(newState).toEqual(expectedState)
    })
    test('resets state after rest action has been dispatched', () => {
      store.dispatch(reset())
      const newState = store.getState()
      const expectedState = {
        secretWord,
        success: false,
        serverError: false,
        guessedWords: []
      }
      expect(newState).toEqual(expectedState)
    })
  })
})
```

## redux actions

### Example #1

index.js
```javascript
import {getLetterMatchCount} from '../helpers'
import axios from 'axios'

export const actionTypes = {
  CORRECT_GUESS: 'CORRECT_GUESS',
  GUESS_WORD: 'GUESS_WORD',
  SET_SECRET_WORD: 'SET_SECRET_WORD',
  RESET: 'RESET',
  SERVER_ERROR: 'SERVER_ERROR'
}

export const guessWord = (guessedWord) => {
  return function (dispatch, getState) {

    const secretWord = getState().secretWord
    const letterMatchCount = getLetterMatchCount(guessedWord, secretWord)

    dispatch({
      type: actionTypes.GUESS_WORD,
      payload: {guessedWord, letterMatchCount}
    })

    if (guessedWord === secretWord) {
      dispatch({
        type: actionTypes.CORRECT_GUESS
      })
    }

  }
}

export const getSecretWord = () => {
  return (dispatch) => {
    return axios.get('http://localhost:3030')
      .then(response => {
        dispatch({
          type: actionTypes.SET_SECRET_WORD,
          payload: response.data
        })
      })
      .catch(error => {
        dispatch({type: actionTypes.SERVER_ERROR})
      })
  }
}

export const reset = () => {
  return (dispatch) => {
    dispatch({
      type: actionTypes.RESET
    })
  }
}
```

<br />

index.test.js
```javascript
import moxios from 'moxios'

import {storeFactory} from '../../test/testUtils'
import {getSecretWord} from './index'

describe('secretWord action creator', () => {
  beforeEach(() => {
    moxios.install()
  })
  afterEach(() => {
    moxios.uninstall()
  })
  let store
  test('adds response word to state', () => {
    const secretWord = 'party'
    const store = storeFactory()

    moxios.wait(() => {
      const request = moxios.requests.mostRecent()
      request.respondWith({
        status: 200,
        response: secretWord
      })
    })

    return store.dispatch(getSecretWord())
      .then(() => {
        const newState = store.getState()
        expect(newState.secretWord).toBe(secretWord)
      })
  })
  test('updates error correctly after 404 error', () => {
    const store = storeFactory()

    moxios.wait(() => {
      const request = moxios.requests.mostRecent()
      request.respondWith({
        status: 404
      })
    })

    return store.dispatch(getSecretWord())
      .then(() => {
        const newState = store.getState()
        expect(newState.serverError).toBe(true)
      })
  })
})
```

<br />

## Reducers

### Example #1

serverReducer.js
```javascript
import {actionTypes} from '../actions'

export default (state = false, action) => {
  switch (action.type) {
    case actionTypes.CORRECT_GUESS:
      return true
    default:
      return state
  }
}
```

<br />

serverReducer.test.js
```javascript
import {actionTypes} from '../actions'
import successReducer from './successReducer'

test('returns default initial state of `false` when no action is passed', () => {
  const newState = successReducer(undefined, {})
  expect(newState).toBe(false)
})

test('returns state of true upon receiving an action of type `CORRECT_GUESS`', () => {
  const newState = successReducer(undefined, {type: actionTypes.CORRECT_GUESS})
  expect(newState).toBe(true)
})
```

<br />

### Example #2

serverErrorReducer.js
```javascript
import {actionTypes} from '../actions'

export default (state = false, action) => {
  switch (action.type) {
    case actionTypes.SERVER_ERROR:
      return true
    default:
      return state
  }
}
```

<br />

serverErrorReducer.test.js
```javascript
import {actionTypes} from '../actions'
import serverErrorReducer from './serverErrorReducer'

test('returns false when initially created', () => {
  const newState = serverErrorReducer(undefined, {})
  expect(newState).toBe(false)
})

test('returns true when action server error passed', () => {
  const newState = serverErrorReducer(undefined, {type: actionTypes.SERVER_ERROR})
  expect(newState).toBe(true)
})
```

<br />

## Helpers

### Example #1

index.js
```javascript
export function getLetterMatchCount(guessedWord, secretWord) {
  const secretLetterSet = new Set(secretWord.split(''))
  const guessedLetterSet = new Set(guessedWord.split(''))
  return [...secretLetterSet].filter(letter => guessedLetterSet.has(letter)).length
}
```

<br />

index.test.js
```javascript
import {getLetterMatchCount} from './index'

describe('getLetterMatchCount', () => {
  const secretWord = 'party'
  test('returns correct count when there are no matching letters', () => {
    const letterMatchCount = getLetterMatchCount('bones', secretWord)
    expect(letterMatchCount).toBe(0)
  })
  test('returns correct count when there are 3 matching letters', () => {
    const letterMatchCount = getLetterMatchCount('train', secretWord)
    expect(letterMatchCount).toBe(3)
  })
  test('returns correct count when there are duplicate letters in the guess', () => {
    const letterMatchCount = getLetterMatchCount('parka', secretWord)
    expect(letterMatchCount).toBe(3)
  })
})
```

<br />

## Extras

### Moxios

integration.test.js
```javascript
import React from 'react'
import {mount} from 'enzyme'
import moxios from 'moxios'
import Root from 'Root'
import App from 'components/App'
import {createMemoryHistory} from "history";

beforeEach(() => {
  moxios.install()
  moxios.stubRequest('http://jsonplaceholder.typicode.com/comments', {
    status: 200,
    response: Array(5).fill({name: 'Comment'})
  })
})

afterEach(() => {
  moxios.uninstall()
})

it('can fetch a list of comments and display them', (done) => {
  const wrapped = mount(
    <Root initialRoute={['/post']}>
      <App />
    </Root>
  )
  wrapped.find('li').at(1).simulate('click')
  wrapped.find('.fetch-comments').simulate('click')
  wrapped.find('li').at(0).simulate('click')

  // introduce a tiny little pause
  moxios.wait(() => {
    wrapped.update()
    expect(wrapped.find('li').length).toEqual(3)
    wrapped.unmount()
    done()
  })
})
```
