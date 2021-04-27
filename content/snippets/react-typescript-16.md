---
title: React typescript 16.8+ (hooks)
---

## Introduction

React is a JavaScript library for building user interfaces. [react docs](https://reactjs.org/docs/getting-started.html)

Typescript docs react reference [typescript docs](https://www.typescriptlang.org/docs/handbook/react.html)

Typescript react cheatsheet [cheatsheet](https://github.com/typescript-cheatsheets/react#reacttypescript-cheatsheets)


## Set up

### Installation 

1. 
```bash
npx create-react-app my-app --template typescript
```
*To add TypeScript to an existing Create React App project, first install it*

```bash
npm install --save typescript @types/node @types/react @types/react-dom @types/jest
```
rename any file to be a TypeScript file (e.g. src/index.js to src/index.tsx)


### tsconfig.json
The config should have the following properties
```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx"
  },
  "include": [
    "src"
  ]
}
```

### index.tsx
The main react file should look similarly to this.

```tsx
import React from 'content/snippets/react-techniques';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```

### App.tsx
The main app react file should look similarly to this

```tsx
import React from 'content/snippets/react-techniques';
import { Provider } from "react-redux";
import { store } from './state'
import RepositoriesList from "./components/RepositoriesList";

function App() {
  return (
    <div className="App">

      <Provider store={store}>
        <div>
          <h1>Search for a package</h1>
          <RepositoriesList />
        </div>
      </Provider>
    </div>
  );
}

export default App;
```

### package.json
```json
{
  "dependencies": {
    "@testing-library/jest-dom": "^5.11.9",
    "@testing-library/react": "^11.2.5",
    "@testing-library/user-event": "^12.7.3",
    "@types/jest": "^26.0.20",
    "@types/node": "^12.20.4",
    "@types/react": "^17.0.2",
    "@types/react-dom": "^17.0.1",
    "@types/react-redux": "7.1.15",
    "axios": "0.21.1",
    "react": "^17.0.1",
    "react-dom": "^17.0.1",
    "react-redux": "7.2.2",
    "react-scripts": "4.0.3",
    "redux": "4.0.5",
    "redux-thunk": "2.3.0",
    "typescript": "^4.2.2",
    "web-vitals": "^1.1.0"
  }
}
```

<br />

## Store (Redux)

Redux is a predictable state container for JavaScript apps. [redux docs, usage with typescript](https://redux.js.org/recipes/usage-with-typescript)

As of React Redux v7.2.3, the react-redux package has a dependency on @types/react-redux,
so the type definitions will be automatically installed with the library. 
Otherwise, you'll need to manually install them yourself 
(typically npm install @types/react-redux ).

### root files
This file gets imported in App.tsx and get inserted into the <Provider store=store />

store.ts
```tsx
import {createStore, applyMiddleware} from "redux";
import thunk from 'redux-thunk'
import reducers from './reducers'

export const store = createStore(reducers, {}, applyMiddleware(thunk))
```

For easier importing throughout the application,
a file is made which contains all store files (reducers, stores, actions)

index.ts
```tsx
export * from './store'
export * as actionCreators from './action-creators'
export * from './reducers'
```

<br />

### ./action-types

index.ts

```typescript
export enum ActionType {
    SEARCH_REPOSITORIES = 'search_repositories',
    SEARCH_REPOSITORIES_SUCCESS = 'search_repositories_success',
    SEARCH_REPOSITORIES_ERROR = 'search_repositories_error',
}
```

<br />


### ./actions
For improved typing a variable is declared, which is of type of all the actions

index.ts
```tsx
import {ActionType} from "../action-types";

interface SearchRepositoriesAction {
    type: ActionType.SEARCH_REPOSITORIES
}

interface SearchRepositoriesSuccessAction {
    type: ActionType.SEARCH_REPOSITORIES_SUCCESS;
    payload: string[];
}


interface SearchRepositoriesErrorAction {
    type: ActionType.SEARCH_REPOSITORIES_ERROR;
    payload: string;
}

export type Action =
    | SearchRepositoriesAction
    | SearchRepositoriesSuccessAction
    | SearchRepositoriesErrorAction
```

<br />

### ./reducers
* ReturnType<Type>: Constructs a type consisting of the return type of function Type. ([ReturnType docs](https://www.typescriptlang.org/docs/handbook/utility-types.html#returntypetype))

index.ts
```tsx
import {combineReducers} from "redux";
import repositoriesReducer from "./repositoriesReducer";

const reducers = combineReducers({
    repositories: repositoriesReducer
})

export default reducers

export type RootState = ReturnType<typeof reducers>
```

<br />

repositoriesReducer.ts
```tsx
import {ActionType} from "../action-types";
import {Action} from "../actions";

interface RepositoriesState {
    loading: boolean;
    error: string | null;
    data: string[];
}

const initialState = {
    loading: false,
    error: null,
    data: []
}

const reducer = (
    state: RepositoriesState = initialState,
    action: Action
): RepositoriesState => {
    switch (action.type) {
        case ActionType.SEARCH_REPOSITORIES:
            return {
                loading: true,
                error: null,
                data: []
            }
        case ActionType.SEARCH_REPOSITORIES_SUCCESS:
            return {
                loading: false,
                error: null,
                data: action.payload
            }
        case ActionType.SEARCH_REPOSITORIES_ERROR:
            return {
                loading: false,
                error: action.payload,
                data: []
            }
        default:
            return state;
    }
}

export default reducer
```

<br />


### ./actions-creators
* Dispatch<Action> helps with code assistance.
  Returning a valid object of type Action from ./actions  
  

```typescript
import axios from 'axios'
import {ActionType} from "../action-types";
import {Action} from "../actions";
import {Dispatch} from "redux";


export const searchRepositories = (term: string) => {
    return async (dispatch: Dispatch<Action>) => {
        dispatch({
            type: ActionType.SEARCH_REPOSITORIES
        })
        try {
            const {data} = await axios.get('https://registry.npmjs.org/-/v1/search', {
                params: {
                    text: term
                }
            })
            const names = data.objects.map((result: any) => {
                return result.package.name
            })
            dispatch({
                type: ActionType.SEARCH_REPOSITORIES_SUCCESS,
                payload: names
            })
        } catch (err) {
            dispatch({
                type: ActionType.SEARCH_REPOSITORIES_ERROR,
                payload: err.message
            })
        }
    }
}
```

<br />

## Hooks

### useTypedSelector
TypedUseSelectorHook needs to know of the state of the store
(`export type RootState = ReturnType<typeof reducers>` from ./store/reducers/index.ts)
```ts
import {useSelector, TypedUseSelectorHook} from "react-redux";
import {RootState} from "../state";

export const useTypedSelector: TypedUseSelectorHook<RootState> = useSelector
```

### useActions
bindActionsCreators need to know about the actions of the store
(`export * as actionCreators from './action-creators'` from ./store/index.ts)

```typescript
import {useDispatch} from "react-redux";
import {bindActionCreators} from "redux";
import {actionCreators} from '../state'

export const useActions = () => {
    const dispatch = useDispatch()

    return bindActionCreators(actionCreators, dispatch)
}
```
<br/>

or 

<br />

*Memoization is an optimization technique which passes a complex function
to be memoized. In memoization, the result is “remembered” when the
same parameters are passed-in subsequently.*

*If we have a function compute 1 + 1, it will return 2. But if it uses memoization,
the next time we run 1’s through the function, it won’t add them up;
it will just remember the answer is 2 
without executing the adding function. ([understanding react usememo](https://www.digitalocean.com/community/tutorials/react-usememo))*

```typescript
import { useDispatch } from 'react-redux'
import { bindActionCreators } from "redux";
import { useMemo } from "content/snippets/react-techniques";
import { actionCreators } from '../state'

export const useActions = () => {

  const dispatch = useDispatch()

  return useMemo(() => {
    return bindActionCreators(actionCreators, dispatch)
  }, [dispatch])
}
```

<br />

## Components
Here it is being shown how to use the previous hooks with typescript assistance

### RepositoriesList.tsx

```tsx
import React, { FormEvent, useState } from 'content/snippets/react-techniques';
import { useActions } from "../hooks/useActions";
import { useSelector } from "react-redux";
import { useTypedSelector } from "../hooks/useTypedSelector";

const RepositoriesList: React.FC = () => {
  const [term, setTerm] = useState('')
  const { searchRepositories } = useActions()
  const { data, error, loading } = useTypedSelector(
    (state) => state.repositories
  )

  const onSubmit = (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault()
    searchRepositories(term)
  }

  return (
    <div>
      <form onSubmit={onSubmit}>
        <input
          value={term}
          onChange={e => setTerm(e.target.value)}
        />
        <button>Search</button>
      </form>
      {
        error && <h3>{error}</h3>
      }
      {
        loading && <h3>Loading...</h3>
      }
      {
        !error && !loading && data.map(name => {
          return <div>{name}</div>
        })
      }
    </div>
  );
};

export default RepositoriesList;
```

<br />

## Use of refs
Here it is being shown how to specify the type of ref that is being handled.
In this case the ref could be HTMLInputElement or null. Inside of use effect, we use a typescript guard,
so that we can access the HTMLInputElement if needed.

UserSearch.tsx

```tsx
import React, { useState, useRef, useEffect } from 'content/snippets/react-techniques';

const users = [
  { name: 'Sarah', age: 20 },
  { name: 'Alex', age: 20 },
  { name: 'Michael', age: 20 }
]

export const UserSearch: React.FC = (props) => {

  const [name, setName] = useState('')
  const [user, setUser] = useState<{ name: string, age: number } | undefined>()
  const inputRef = useRef<HTMLInputElement | null>(null)

  useEffect(() => {
    if (!inputRef.current) {
      return
    }
    inputRef.current.focus()
  }, [])

  const onClick = () => {
    const foundUser = users.find((user) => {
      return user.name === name
    })

    setUser(foundUser)
  }

  return (
    <div>
      User Search
      <input
        ref={inputRef}
        value={name}
        onChange={(e) => {
          setName(e.target.value)
        }}
      />
      <button
        onClick={onClick}
      >
        Find User
      </button>
      <div>
        {user?.name}
        {user?.age}
      </div>
    </div>
  )
};
```

<br />

## Use of props
We substitute the usage of `prop-types` for a typed version of it, by using 
`React.FC<Props>`. This give us the ability to use interface instead of the prop-types way.

*Display name is mainly used within react error messaging.
  This is why it is mentioned to be valuable for debugging.
  If no name can be derived the error messages default to say
  Component which is extremely difficult to debug, when
  you have any more than 1 component in your project.* [Stack overflow question](https://stackoverflow.com/questions/41581130/what-is-react-component-displayname-is-used-for)

Child.tsx

```tsx
import React from 'content/snippets/react-techniques';

interface ChildProps {
  color: string;
  onClick: () => void
}

export const Child = ({ color, onClick }: ChildProps) => {
  return (
    <div>
      {color}
      <button onClick={onClick}>
        Click me
      </button>
    </div>
  );
};

export const ChildAsFc: React.FC<ChildProps> = ({ color, onClick, children }) => {
  return <div>
    {color}
    <button onClick={onClick}>
      Click me
    </button>
    {
      children
    }
  </div>
}


ChildAsFc.displayName = ''
```

<br />

Parent.tsx

```tsx
import React from 'content/snippets/react-techniques';
import { Child, ChildAdFc } from "./Child";


const Parent = () => {
  return (
    <ChildAdFc
      color="red"
      onClick={() => {
        console.log('Clicked')
      }}
    >

    </ChildAdFc>
  );
};


export default Parent;
```


## Use of state

use `useState<State>` for better typing assistance

GuestList.tsx

```tsx
import React from 'content/snippets/react-techniques'
import { useState } from "content/snippets/react-techniques";

const GuestList: React.FC = () => {
  const [name, setName] = useState('')
  const [guests, setGuests] = useState<string[]>([])

  const onClick = () => {
    setName('')
    setGuests([...guests, name])
  }

  return <div>
    <h3>Guest LIst</h3>
    <ul>
      {
        guests.map(guest => <li key={guest}>{guest}</li>)
      }
    </ul>
    <input
      value={name}
      onChange={(e) => {
        setName(e.target.value)
      }}
    />
    <button
      onClick={() => {
        onClick()
      }}
    >Add Guest
    </button>
  </div>
}

export default GuestList
```

<br />

UserSearch.tsx

```tsx
import React, { useState } from 'content/snippets/react-techniques';

const users = [
  { name: 'Sarah', age: 20 },
  { name: 'Alex', age: 20 },
  { name: 'Michael', age: 20 }
]

export const UserSearch: React.FC = (props) => {

  const [name, setName] = useState('')
  const [user, setUser] = useState<{ name: string, age: number } | undefined>()

  const onClick = () => {
    const foundUser = users.find((user) => {
      return user.name === name
    })

    setUser(foundUser)
  }

  return (
    <div>
      User Search
      <input
        value={name}
        onChange={(e) => {
          setName(e.target.value)
        }}
      />
      <button
        onClick={onClick}
      >
        Find User
      </button>
      <div>
        {user?.name}
        {user?.age}
      </div>
    </div>
  )
};
```

## Use of events

For better event type assistance, type the event with the corresponding HTML Event. Hover over the prop using
a text editor to know which event you need to type (I am using Intellij IDEA)

EventComponent.tsx

```tsx
import React from 'content/snippets/react-techniques';

const EventComponent: React.FC = () => {
  const onChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    console.log(event)
  }

  const onDragStart: React.DragEventHandler<HTMLDivElement> = (event) => {
    console.log(event)
  }

  return (
    <div>
      <input
        onChange={onChange}
      />
      <div
        draggable
        onDragStart={onDragStart}
      >
        Drag me!
      </div>
    </div>
  );
};

export default EventComponent;
```


## More examples

### Reuse of useTypedSelector


use-cumulative-hook.ts
```typescript
import {useTypedSelector} from "./use-typed-selector";

export const useCumulativeCode = (cellId: string) => {
    return useTypedSelector((state) => {
        const {data, order} = state.cells
        const orderedCells = order.map(id => data[id])


        const showFunc =
            `
            import _React from 'react'
            import _ReactDOM from 'react-dom'
            var show = (value) => {
                const root = document.querySelector('#root')
                
                if (typeof value === 'object') {
                    if (value.$$typeof && value.props) {
                        _ReactDOM.render(value, root)
                    } else {
                        root.innerHTML = JSON.stringify(value);
                    }
                } else {
                    root.innerHTML = value;   
                }
            }
            `
        const showFuncNoop = 'var show = () => {}'
        const cumulativeCode = [


        ]
        for (let c of orderedCells) {
            if (c.type === 'code') {
                if (c.id === cellId) {
                    cumulativeCode.push(showFunc)
                } else {
                    cumulativeCode.push(showFuncNoop)
                }
                cumulativeCode.push(c.content)
            }
            if (c.id === cellId) {
                break
            }
        }
        return cumulativeCode
    }).join('\n')
}
```

<br />

### usage of useActions and useTypedSelector #1
CellList.tsx

```tsx
import React, { useEffect } from 'content/snippets/react-techniques';
import { useTypedSelector } from "../hooks/use-typed-selector";
import CellListItem from "./cell-list-item";
import AddCell from "./add-cell";
import './cell-list.css'
import { useActions } from "../hooks/use-actions";

const CellList: React.FC = () => {
  const cells = useTypedSelector(({ cells: { order, data } }) => {
    return order.map(id => {
      return data[id]
    })
  })

  const { fetchCells, saveCells } = useActions()
  useEffect(() => {
    fetchCells()
  }, [])


  const renderedCells = cells.map(cell => {
    return <React.Fragment key={cell.id}>
      <CellListItem
        key={cell.id}
        cell={cell}
      />
      <AddCell previousCellId={cell.id} />
    </React.Fragment>
  })

  return (
    <div className="cell-list">
      <AddCell
        forceVisible={cells.length === 0}
        previousCellId={null}
      />
      {renderedCells}
    </div>
  );
};

export default CellList;
```

<br />

### Typed JSX.Element variable
CellItem.tsx

```tsx
import React from 'content/snippets/react-techniques';
import './cell-list-item.css'
import { Cell } from "../state";
import CodeCell from "./code-cell";
import TextEditor from "./text-editor";
import ActionBar from "./action-bar";

interface CellListItemProps {
  cell: Cell
}


const CellListItem: React.FC<CellListItemProps> = ({ cell }) => {

  let child: JSX.Element


  if (cell.type === 'code') {
    child = <>
      <div className="action-bar-wrapper">
        <ActionBar id={cell.id} />
      </div>
      <CodeCell cell={cell} />
    </>
  } else {
    child = <>
      <TextEditor cell={cell} />
      <ActionBar id={cell.id} />
    </>
  }

  return (
    <div className="cell-list-item">
      {child}
    </div>
  );
};

export default CellListItem;
```

<br />

### Putting all  together

Cell.tsx

```tsx
import React from 'content/snippets/react-techniques'
import { useEffect } from 'content/snippets/react-techniques'
import CodeEditor from "../components/code-editor";
import Preview from "../components/preview";
import Resizable from "./resizable";
import { Cell } from "../state";
import { useActions } from "../hooks/use-actions";
import { useTypedSelector } from "../hooks/use-typed-selector";
import './code-cell.css'
import { useCumulativeCode } from "../hooks/use-cumulative-hook";

interface CodeCellProps {
  cell: Cell
}

const CodeCell: React.FC<CodeCellProps> = ({ cell }) => {

  const { updateCell, createBundle } = useActions()
  const bundle = useTypedSelector((state) => state.bundles[cell.id])
  const cumulativeCode = useCumulativeCode(cell.id)

  useEffect(() => {
    if (!bundle) {
      createBundle(cell.id, cumulativeCode)
      return
    }

    const timer = setTimeout(async () => {
      createBundle(cell.id, cumulativeCode)
    }, 750)
    return () => {
      clearTimeout(timer)
    }
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, [cumulativeCode, cell.id, createBundle])

  return (
    <Resizable direction="vertical">
      <div
        style={{
          height: 'calc(100% - 10px)',
          display: 'flex',
          flexDirection: 'row'
        }}
      >
        <Resizable direction="horizontal">
          <CodeEditor
            initialValue={cell.content}
            onChange={(value) => updateCell(cell.id, value)}
          />
        </Resizable>
        <div className="progress-wrapper">
          {
            !bundle || bundle.loading
              ?
              <div className="progress-cover">
                <progress
                  className="progress is-small is-primary"
                  max="100"
                >
                  Loading
                </progress>
              </div>
              : <Preview
                code={bundle.code}
                bundlingStatus={bundle.err}
              />
          }
        </div>
      </div>
    </Resizable>
  )
}

export default CodeCell
```

<br />

### Another reducer
cell-reducer.ts
```ts

import produce from 'immer'
import {ActionType} from "../action-types";
import {Action} from "../actions";
import {Cell} from "../cell";

interface CellsState {
    loading: boolean;
    error: string | null;
    order: string[];
    data: {
        [key: string]: Cell
    }
}

const initialState: CellsState = {
    loading: false,
    error: null,
    order: [],
    data: {}
}

const reducer = produce((state: CellsState = initialState, action: Action) => {
    switch (action.type) {
        case ActionType.SAVE_CELLS_ERROR:
            state.error = action.payload
            return state;
        case ActionType.FETCH_CELLS:
            state.loading = true
            state.error = null
            return state
        case ActionType.FETCH_CELLS_COMPLETE:
            state.order = action.payload.map(cell => cell.id)
            state.data = action.payload.reduce((acc, cell) => {
                acc[cell.id] = cell
                return acc
            }, {} as CellsState['data'])
            return state
        case ActionType.FETCH_CELLS_ERROR:
            state.loading = false
            state.error = action.payload
            return state
        case ActionType.DELETE_CELL:
            delete state.data[action.payload]
            state.order = state.order.filter(id => id !== action.payload)
            return state
        case ActionType.INSERT_CELL_AFTER:
            const cell: Cell = {
                content: '',
                type: action.payload.type,
                id: randomId()
            }

            state.data[cell.id] = cell

            const findIndex = state.order.findIndex(id => id === action.payload.id)

            if (findIndex < 0) {
                state.order.unshift(cell.id)
            } else {
                state.order.splice(findIndex + 1, 0, cell.id)
            }

            return state;
        case ActionType.MOVE_CELL:
            const {direction} = action.payload
            const index = state.order.findIndex((id) => id === action.payload.id)
            const targetIndex = direction === 'up' ? index -1 : index + 1

            if (targetIndex < 0 || targetIndex > state.order.length -1) {
                return state
            }

            state.order[index] = state.order[targetIndex]
            state.order[targetIndex] = action.payload.id

            return state
        case ActionType.UPDATE_CELL:
            const {id, content} = action.payload
            state.data[id].content = content
            return state
        default:
            return state;
    }
})

const randomId = () => {
    return Math.random().toString(36).substring(2, 5)
}

export default reducer
```

<br />

### More actions

./actions/index.ts
```ts
import {Dispatch} from 'redux'
import {Action, DeleteCellAction, Direction, InsertCellAfterAction, MoveCellAction, UpdateCellAction} from "../actions";
import {ActionType} from "../action-types";
import {Cell, CellTypes} from "../cell";
import bundle from "../../bundler";
import axios from "axios";
import {RootState} from "../reducers";

export const updateCell = (id: string, content: string): UpdateCellAction  => {
    return {
        type: ActionType.UPDATE_CELL,
        payload: {
            id,
            content
        }
    }
}

export const deleteCell = (id: string): DeleteCellAction => {
    return {
        type: ActionType.DELETE_CELL,
        payload: id
    }
}

export const moveCell = (id: string, direction: Direction): MoveCellAction => {
    return {
        type: ActionType.MOVE_CELL,
        payload: {
            id,
            direction
        }
    }
}

export const insertCellAfter = (id: string | null, cellType: CellTypes): InsertCellAfterAction => {
    return {
        type: ActionType.INSERT_CELL_AFTER,
        payload: {
            id,
            type: cellType
        }
    }
}

export const createBundle = (cellId: string, input: string) => {
    return async (dispatch: Dispatch<Action>) => {
        dispatch({
            type: ActionType.BUNDLE_START,
            payload: {
                cellId
            }
        })

        const result = await bundle(input)

        dispatch({
            type: ActionType.BUNDLE_COMPLETE,
            payload: {
                cellId,
                bundle: result
            }
        })
    }
}

export const fetchCells = () => {
    return async (dispatch: Dispatch<Action>) => {
        dispatch({
            type: ActionType.FETCH_CELLS
        })

        try {
            const {data}: {data: Cell[]} = await axios.get('/cells')
            dispatch({
                type: ActionType.FETCH_CELLS_COMPLETE,
                payload: data
            })
        } catch (err) {
            dispatch({
                type: ActionType.FETCH_CELLS_ERROR,
                payload: err.message
            })
        }
    }
}

export const saveCells = () => {
    return async (dispatch: Dispatch<Action>, getState: () => RootState) => {
        const {cells: {data, order}} = getState()
        const cells = order.map(id => data[id])
        try {
            await axios.post('/cells', {cells})
        } catch (err) {
            dispatch({
                type: ActionType.SAVE_CELLS_ERROR,
                payload: err.message
            })
        }
    }
}
```

<br />

## Extras


### Reuse of interfaces across app
As a recommendation, you could declare interfaces inside of the store and then export them from index.ts
Cell.ts
```ts
export type CellTypes = 'code' | 'text'
export interface Cell {
    id: string;
    type: CellTypes;
    content: string;
}
```

index.ts
```ts
export * from './reducers'
export * from './store'
export * from './cell'
export * as actionCreators from './action-creators'
```

### Custom middleware

save-cells.ts

```ts
export const saveCells = () => {
    return async (dispatch: Dispatch<Action>, getState: () => RootState) => {
        const {cells: {data, order}} = getState()
        const cells = order.map(id => data[id])
        try {
            await axios.post('/cells', {cells})
        } catch (err) {
            dispatch({
                type: ActionType.SAVE_CELLS_ERROR,
                payload: err.message
            })
        }
    }
}
```

persist-middleware.ts
```ts
import {Action} from "../actions";
import {Dispatch} from "redux";
import {ActionType} from "../action-types";
import {saveCells} from "../action-creators";
import {RootState} from "../reducers";

export const persistMiddleware = ({dispatch, getState}: {
  dispatch: Dispatch<Action>,
  getState: () => RootState
}) => {
  let timer: any;
  return (next: (action: Action) => void) => {
    return (action: Action) => {
      next(action)
      if (
        [
          ActionType.MOVE_CELL,
          ActionType.UPDATE_CELL,
          ActionType.INSERT_CELL_AFTER,
          ActionType.DELETE_CELL
        ].includes(action.type)
      ) {
        if (timer) {
          clearTimeout(timer)
        }
        timer = setTimeout(() => {
          saveCells()(dispatch, getState)
        }, 250)
      }

    }
  }
}
```

Modify store.ts in ./state/store.ts like the next example

store.ts
```ts
import {createStore, applyMiddleware} from "redux";
import thunk from 'redux-thunk'
import reducers from './reducers'
import {ActionType} from "./action-types";
import {persistMiddleware} from "./middlewares/persist-middleware";

export const store = createStore(reducers, {}, applyMiddleware(thunk, persistMiddleware))
```

### Usage of iframe in React
Preview.tsx

```tsx
import React, { useEffect, useRef } from 'content/snippets/react-techniques';
import './preview.css'

interface PreviewProps {
  code: string;
  bundlingStatus: string;
}

const html = `
        <html>
          <head>
          
            <style>
              html { background-color: white; }
            </style>
          </head>
          <body>
            <div id="root">
            
            </div>
            <script>
              const handleError = (err) => {
                 const root = document.querySelector('#root')
                 root.innerHTML = '<div style="color: red;"><h4>Runtime Error</h4>' + err + '</div>'
                 console.error(err)
              }
              window.addEventListener('error', (event) => {
                event.preventDefault()
                handleError(event.error)
              })
              window.addEventListener('message', event => {
               try { 
                  eval(event.data)
               } catch (err) {
                  handleError(err)
               }
              }, false)
            </script>
          
          </body>
        </html>
        `

const Preview: React.FC<PreviewProps> = ({ code, bundlingStatus }) => {
  const iframeRef = useRef<any>()

  useEffect(() => {
    // reset iframe to initial html doc
    iframeRef.current.srcdoc = html
    setTimeout(() => {
      iframeRef.current.contentWindow.postMessage(code, '*')
    }, 50)
  }, [code])

  return (
    <div className="preview-wrapper">
      <iframe
        title="preview"
        ref={iframeRef}
        sandbox="allow-scripts"
        srcDoc={html}
      />
      {
        bundlingStatus && <div className="preview-error">{bundlingStatus}</div>
      }
    </div>
  );
};

export default Preview;
```

<br />

### Resizable component
Resizable.tsx

```tsx
import React from 'content/snippets/react-techniques';
import { useEffect, useState } from "content/snippets/react-techniques";
import { ResizableBox, ResizableBoxProps } from "react-resizable";
import './resizable.css'

interface ResizableProps {
  direction: 'horizontal' | 'vertical'
}

const Resizable: React.FC<ResizableProps> = ({ direction, children }) => {
  let resizableProps: ResizableBoxProps
  const [innerHeight, setInnerHeight] = useState(window.innerHeight)
  const [innerWidth, setInnerWidth] = useState(window.innerWidth)
  const [width, setWidth] = useState(window.innerWidth * 0.75)

  useEffect(() => {
    let timer: any;
    const listener = () => {
      if (timer) {
        clearTimeout(timer)
      }
      setTimeout(() => {
        setInnerHeight(window.innerHeight)
        setInnerWidth(window.innerWidth)
        if (window.innerWidth * 0.75 < width) {
          setWidth(window.innerWidth * 0.75)
        }
      }, 100)
    }
    window.addEventListener('resize', listener)
    return () => {
      window.removeEventListener('resize', listener)
    }
  }, [width])

  if (direction === 'horizontal') {
    resizableProps = {
      className: 'resize-horizontal',
      height: Infinity,
      minConstraints: [innerWidth * 0.2, Infinity],
      maxConstraints: [innerWidth * 0.75, Infinity],
      width,
      resizeHandles: ['e'],
      onResizeStop: (event, data) => {
        setWidth(data.size.width)
      }
    }
  } else {
    resizableProps = {
      height: 300,
      minConstraints: [Infinity, 24],
      maxConstraints: [Infinity, innerHeight * 0.90],
      width: Infinity,
      resizeHandles: ['s'],
    }
  }

  return (
    <ResizableBox
      {...resizableProps}
    >
      {children}
    </ResizableBox>
  );
};

export default Resizable;
```


### Text Editor
package.json
````json
{
  "@monaco-editor/react": "3.7.5",
  "immer": "^8.0.1",
  "jscodeshift": "0.11.0",
  "localforage": "^1.9.0",
  "monaco-editor": "0.22.3",
  "monaco-jsx-highlighter": "0.0.15"
}
````
<br />

TextEditor.tsx

```tsx
import MDEditor from '@uiw/react-md-editor'
import React from "content/snippets/react-techniques";
import { useState, useEffect, useRef } from 'content/snippets/react-techniques'
import './text-editor.css'
import { Cell } from "../state";
import { useActions } from "../hooks/use-actions";

interface TextEditorProps {
  cell: Cell;
}


const TextEditor: React.FC<TextEditorProps> = ({ cell }) => {
  const [editing, setEditing] = useState(false)
  const ref = useRef<HTMLDivElement | null>(null)
  const { updateCell } = useActions()


  useEffect(() => {
    const listener = (event: MouseEvent) => {
      if (ref.current && event.target && ref.current?.contains(event.target as Node)) {
        // console.log('element clicked on is inside editor')
        return
      }
      // console.log('element clicked on is not inside editor')
      setEditing(false)
    }
    document.addEventListener('click', listener, { capture: true })
    return () => {
      document.removeEventListener('click', listener, { capture: true })
    }
  }, [])

  if (editing) {
    return (
      <div
        ref={ref}
        className="text-editor"
      >
        <MDEditor
          value={cell.content}
          onChange={(v) => {
            updateCell(cell.id, v || '')
          }}
        />
      </div>
    )
  }

  return <div
    className="text-editor card"
    onClick={() => {
      setEditing(true)
    }}
  >
    <div className="card-content">
      <MDEditor.Markdown
        source={cell.content || 'Click to edit'}
      />
    </div>
  </div>
}

export default TextEditor
```
