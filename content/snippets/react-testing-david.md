---
title: BDD React testing (David joseph)
---

## Introduction

BDD = Given, when and then

package.json
```json
{
  "name": "lootcheck",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@testing-library/jest-dom": "^5.11.9",
    "@testing-library/react": "^11.2.3",
    "@testing-library/user-event": "^12.6.2",
    "@wojtekmaj/enzyme-adapter-react-17": "^0.4.1",
    "enzyme": "^3.11.0",
    "jest-cli": "^26.6.3",
    "node-fetch": "^2.6.1",
    "react": "^17.0.1",
    "react-bootstrap": "^1.4.3",
    "react-dom": "^17.0.1",
    "react-redux": "^7.2.2",
    "react-scripts": "4.0.1",
    "react-test-renderer": "^17.0.1",
    "redux": "^4.0.5",
    "redux-thunk": "^2.3.0",
    "sfcookies": "^1.0.2",
    "web-vitals": "^0.2.4"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
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
    "fetch-mock": "^9.11.0",
    "redux-mock-store": "^1.5.4"
  }
}
```

<br />

setup.js
```javascript
import Enzyme from 'enzyme';
import Adapter from '@wojtekmaj/enzyme-adapter-react-17';

Enzyme.configure({ adapter: new Adapter() });
```

<br />



## Components

### Example #1

App.js
```jsx
import React, {Component} from 'react';
import Wallet from "./Wallet";
import Loot from "./Loot";

class App extends Component {
  render() {
    return (
      <div>
        <h2>Loot check</h2>
        <hr />
        <Wallet />
        <Loot />
      </div>
    );
  }
}

export default App;
```

<br />

App.test.js
```jsx
import React from 'react'
import {shallow} from 'enzyme'
import App from './App'

const app = shallow(<App />)

describe('App', () => {
  it('renders correctly', () => {
    expect(app).toMatchSnapshot()
  })

  it('initializes the `state` with an empty list of gifts', () => {
    expect(app.state().gifts).toEqual([])
  })

  describe('when clicking the `add-gift` button', ()  => {
    const id = 1
    beforeEach(() => {
      app.find('.btn-add').simulate('click')
    })

    afterEach(() => {
      app.setState({gifts: []})
    })

    it('adds a new gift to `state`', () => {
      expect(app.state().gifts).toEqual([{id}])
    })

    it('adds a new gift to the rendered list', () => {
      expect(app.find('.gift-list').children().length).toEqual(1)
    })

    it('creates a Gift component', () => {
      expect(app.find('Gift').exists()).toBe(true)
    })

    describe('and the user wants to remove the added gift', () => {
      beforeEach(() => {
        app.instance().removeGift(id)
      })

      it('removes the gift from `state`', () => {
        expect(app.state().gifts).toEqual([])
      })
    })
  })
})


// BDD -> Given, when and then

// Example -> Given notes when deleting then remove a note
```

<br />

### Example #2


Loot.js
```jsx
import React, {Component} from 'react';
import {connect} from 'react-redux'
import {fetchBitcoin} from "../actions/bitcoin";
import PropTypes from 'prop-types'

export class Loot extends Component {

  constructor(props) {
    super(props);
  }

  componentDidMount() {
    this.props.fetchBitcoin()
  }

  computeBitcoin() {
    const { bitcoin } = this.props

    if (Object.keys(bitcoin).length === 0) return ''

    return this.props.balance / parseInt(bitcoin.bpi.USD.rate.replace(',', ''), 10)
  }

  render() {
    return (
      <h3>Bitcoin balance: {this.computeBitcoin()}</h3>
    );
  }
}

const mapStateToProps = (state) => {
  return {
    bitcoin: state.bitcoin,
    balance: state.balance
  }
}

Loot.propTypes = {
  fetchBitcoin: PropTypes.func.isRequired
}

export default connect(mapStateToProps, {fetchBitcoin})(Loot);
```

<br />

Loot.test.js
```javascript
import React from 'react'
import {shallow, mount} from 'enzyme'
import {Loot} from './Loot'

describe('Loot', () => {
  const mockFetchBitcoin = jest.fn()
  let props = {balance: 10, bitcoin: {}, fetchBitcoin: () => {}}
  let loot = shallow(<Loot {...props}/>)

  it ('renders properly', () => {
    expect(loot).toMatchSnapshot()
  })

  describe('when mounted', () => {
    beforeEach(() => {
      loot = mount(<Loot {...props} fetchBitcoin={mockFetchBitcoin}/>)
    })

    it('dispatches the `fetchBitcoin()` method it receives from props', () => {
      expect(mockFetchBitcoin).toHaveBeenCalled()
    })
  })

  describe('when there are valid bitcoin props', () => {
    beforeEach(() => {
      props = { balance: 10, bitcoin: { bpi: {USD: {rate: '1,000'}}}, fetchBitcoin: () => {}}
      loot = shallow(<Loot {...props} />)
    })

    it('displays the correct bitcoin value', () => {
      expect(loot.find('h3').text()).toEqual('Bitcoin balance: 0.01')
    })
  })
})
```

<br />

### Example #3

Wallet.js
```javascript
import React, {Component} from 'react';
import {connect} from 'react-redux'
import {deposit, withdraw} from '../actions/balance'
import PropTypes from 'prop-types'

export class Wallet extends Component {
  constructor(props) {
    super(props);

    this.state = {
      balance: undefined
    }
  }

  updateBalance = (event) => {
    this.setState({
      balance: parseInt(event.target.value, 10)
    })
  }

  handleClick = () => {
    this.props.deposit(this.state.balance)
  }

  handleWithdraw = () => {
    this.props.withdraw(this.state.balance)
  }

  render() {
    return (
      <div>
        <h3 className="balance">
          Wallet balance: {this.props.balance}
        </h3>
        <br/>
        <input className="input-wallet" onChange={this.updateBalance}/>
        <button
          onClick={() => {
            this.handleClick()
          }}
          className="btn-deposit"
        >
          Deposit
        </button>
        <button
          onClick={() => {
            this.handleWithdraw()
          }}
          className="btn-withdraw"
        >
          Deposit
        </button>
      </div>
    );
  }
}

const mapStateToProps = (state) => {
  return {
    balance: state.balance
  }
}

Wallet.propTypes = {
  deposit: PropTypes.func.isRequired
}

export default connect(mapStateToProps, {deposit, withdraw})(Wallet);
```

<br />

Wallet.test.js

```javascript
import React from 'react'
import {shallow} from 'enzyme'
import {Wallet} from './Wallet'

describe('Wallet', () => {
  const mockDeposit = jest.fn()
  const mockWithdraw = jest.fn()
  const props = {balance: 20, deposit: mockDeposit, withdraw: mockWithdraw}
  const wallet = shallow(<Wallet {...props} />)

  it('renders properly', () => {
    expect(wallet).toMatchSnapshot()
  })

  it ('displays the balance from props', () => {
    expect(wallet.find('.balance').text()).toEqual('Wallet balance: 20')
  })

  it('creates an input to deposit or withdraw from the balance', () => {
    expect(wallet.find('.input-wallet').exists()).toBe(true)
  })

  describe('when the user types into the wallet input', () => {
    const userBalance = '25'
    beforeEach(() => {
      wallet.find('.input-wallet').simulate('change', {target: {value: userBalance}})
    })

    it('updates the local wallet balance in `state` and converts it to', () => {
      expect(wallet.state().balance).toEqual(parseInt(userBalance, 10))
    })

    describe('and the user wants to make a deposit', () => {
      beforeEach(() => {
        wallet.find('.btn-deposit').simulate('click')
      })

      it('dispatches the `deposit()` it receives the props with the local balance', () => {
        expect(mockDeposit).toHaveBeenCalledWith(parseInt(userBalance, 10))
      })
    })

    describe('and the user wants to make a withdrawal', () => {
      beforeEach(() => {
        wallet.find('.btn-withdraw').simulate('click')
      })

      it('dispatches the `withdraw()` it receives the props with the local balance', () => {
        expect(mockWithdraw).toHaveBeenCalledWith(parseInt(userBalance, 10))
      })
    })
  })

})
```

<br />

## Reducers

### Index.js

index.js
```javascript
import {combineReducers} from "redux";
import balance from './balance'
import bitcoin from './bitcoin'

export default combineReducers({balance, bitcoin})
```

<br />


index.test.js
```javascript
import rootReducer from './index'

describe('rootReducer', () => {
  it ('initializes the default state', () => {
    expect(rootReducer({}, {})).toEqual({balance: 0, bitcoin: {}})
  })
})
```

<br />

### Example #2

bitcoin.js
```javascript
import {FETCH_BITCOIN} from "../actions/constants";

const bitcoin = (state = {}, action) => {
  switch(action.type) {
    case FETCH_BITCOIN:
      return action.bitcoin
    default:
      return state
  }
}

export default bitcoin
```

<br />

bitcoin.test.js
```javascript
import bitcoinReducer from './bitcoin'
import {FETCH_BITCOIN} from "../actions/constants";

describe('bitcoinReducer', () => {
  const bitcoinData = { bpi: 'bitcoin price index'}

  it ('fetches and sets the bitcoin data', () => {
    expect(bitcoinReducer({}, {type: FETCH_BITCOIN, bitcoin: bitcoinData}))
      .toEqual(bitcoinData)
  })
})
```

<br />

### Example #3

balance.js
```javascript
import * as constants from '../actions/constants'
import {read_cookie, bake_cookie} from 'sfcookies'

const BALANCE_COOKIE = 'BALANCE_COOKIE'

const balance = (state = 0, action) => {
  let balance
  switch (action.type){
    case constants.SET_BALANCE:
      balance = action.balance
      break;
    case constants.DEPOSIT:
      balance = state + action.deposit
      break;
    case constants.WITHDRAW:
      balance = state - action.withdraw
      break;
    default:
      balance = parseInt(read_cookie(BALANCE_COOKIE), 10) || state
      break;
  }
  bake_cookie(BALANCE_COOKIE, balance)
  return balance
}

export default balance
```

<br />

balance.test.js
```javascript
import balanceReducer from './balance'
import balanceReducer2 from './balance'
import * as constants from '../actions/constants'

describe('balanceReducer', () => {
  describe('when initializaing', () => {
    const balance = 10

    it('sets a balance', () => {
      expect(balanceReducer(undefined, {type: constants.SET_BALANCE, balance}))
        .toEqual(balance)
    })

    describe('then re-initializing', () => {
      it('reads the balance from cookies', () => {
        expect(balanceReducer2(undefined, {})).toEqual(balance)
      })
    })
  })

  it('deposits into the balance', () => {
    const deposit = 10
    const initialState = 5

    expect(balanceReducer(initialState, {type: constants.DEPOSIT, deposit}))
      .toEqual(initialState + deposit)
  })

  it('withdraws from the balance', () => {
    const withdraw = 10
    const initialState = 20

    expect(balanceReducer(initialState, {type: constants.WITHDRAW, withdraw}))
      .toEqual(initialState - withdraw)
  })

})
```

<br />

## Actions

constants.js
```javascript
export const SET_BALANCE = 'SET_BALANCE'
export const DEPOSIT = 'DEPOSIT'
export const WITHDRAW = 'WITHDRAW'
export const FETCH_BITCOIN = 'FETCH'
```


### Example #1

bitcoin.js
```javascript
import {FETCH_BITCOIN} from "./constants";

export const fetchBitcoin = () => {
  return dispatch => {
    return fetch('https://api.coindesk.com/v1/bpi/currentprice.json')
      .then(response => response.json())
      .then(json => {
        dispatch({
          type: FETCH_BITCOIN,
          bitcoin: json
        })
      })
  }
}
```

<br />

bitcoin.test.js
```javascript
// https://api.coindesk.com/v1/bpi/currentprice.json
import configureMockStore from 'redux-mock-store'
import thunk from "redux-thunk";
import fetchMock from 'fetch-mock'
import {FETCH_BITCOIN} from './constants'
import {fetchBitcoin} from './bitcoin'

const createMockStore = configureMockStore([thunk])
const store = createMockStore({ bitcoin: {} })
const mockResponse = {body: {bpi: 'bitcoin price index' }}

fetchMock.get('https://api.coindesk.com/v1/bpi/currentprice.json', mockResponse)

it('creates an async action to fetch the bitcoin value', () => {
  const expectedActions = [{bitcoin: mockResponse.body, type: FETCH_BITCOIN}]
  return store.dispatch(fetchBitcoin()).then(() => {
    expect(store.getActions()).toEqual(expectedActions)
  })
})
```

<br />


### Example #2

balance.js
```javascript
import * as constants from './constants'

export const setBalance = (balance) => {
  return {
    type: constants.SET_BALANCE,
    balance
  }
}

export const deposit = deposit => {
  return {
    type: constants.DEPOSIT,
    deposit
  }
}

export const withdraw = withdraw => {
  return {
    type: constants.WITHDRAW,
    withdraw
  }
}
```

<br />

balance.test.js
```javascript
import * as constants from './constants'
import * as actions from './balance'

it('creates an action to set the balance', () => {
  const balance = 0
  const expectedAction = {type: constants.SET_BALANCE, balance}
  expect(actions.setBalance(balance)).toEqual(expectedAction)
})

it('creates an action to deposit into the balance', () => {
  const deposit = 10
  const expectedAction = {type: constants.DEPOSIT, deposit}
  expect(actions.deposit(deposit)).toEqual(expectedAction)
})
it('creates an action to withdraw from the balance', () => {
  const withdraw = 10
  const expectedAction = {type: constants.WITHDRAW, withdraw}
  expect(actions.withdraw(withdraw)).toEqual(expectedAction)
})
```

<br />
