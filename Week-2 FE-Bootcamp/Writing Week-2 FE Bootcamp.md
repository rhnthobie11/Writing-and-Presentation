# Writing Week-2 FE Bootcamp

## React.js Lanjutan
**Prop Types**
- merupakan sebuah library yang dapat membantu untuk **memeriksa data props** yang di kirim agar sesuai dengan ekspetasi

sebelum itu kita harus menginstall nya dengan cara
>npm install prop-types

contoh kasus prop types uamg sesuai
```js
import PropTypes from 'prop-types'; //lakukan import propytyprd

function Header(props) {
    return (
        <>
            <h2>Nama : {props.nama}</h2>
            <h2>Usia : {props.usia}</h2>
        </>
    )
}

Header.propTypes{
    nama: PropTypes.string,
    usia: PropTypes.number
}
```

![proptypes](img/proptypes.jpeg)

berikut pesan error apabila data yang dimasukkan tidak sesuai permintaan.

**Hooks**
- Hooks fitur yang baru dikenalkan di ReactJS pada tahun 2018
- Hooks berfungsi untuk memudahkan pengguna *functional components* agar bisa menggunakan state, dan lifecycle lainnya
- sebelumnya, state (setState) dan lifeCycle (componentDidMount, componentDidUpdate) hanya bisa digunakan di **class component**, tetapi dengan adanya hooks kita bisa menggunkannya di **functional component**
- Hooks yang sering digunakan :
    - useState dan useEffect.

### perbedaan penulisan functional component dengan class component
- Functional Component
```js
import {usetState} from 'react' ;

function app () {
    const [nama, setNama] = useState("Thobie");

    return(
        <>
            <h1>Hello, {nama}</h1>
        </>
    )
}
// OUTPUT 
// Hello, Thobie
```

- Class Component
```js
class component extends React.Component {
    constructor(props) {
        super(props);
        this.state = {nama : "Thobie"};
    }

    render() {
        return <h1>Hello, {this.state.nama}</h1>
    }
}
// OUTPUT
// Hello, Thobie
```

dari perbandingan code diatas dapat kita lihat bahwa dengan **menggunakan** functional component dan Hooks. code yang diketikkan terlihat lebih pendek dan mudah dipahami

useState Syntax Structure

![structure-useState](img/useState-strctr.jpeg)

 ### cara penggunaan useState Hooks
1. import useState dari react `import { useState } from 'react' ; `
2. syntax useState `const [nama, setNama] = useState ('thobie') `
3. memanggil data `<p> halo, saya {nama}</p>`
4. update data `<button onClick={ setNama('raihan')}> ubah </button> `
5. contoh

```js
import { useState } from 'react' ;

function App() {
    const [nama, setNama] = useState('thobie')

    return(
        <>
            <p>halo, saya {name}</p>
            <button onClick={ () => setNama('raihan')}> ganti </button>
    )
};

// OUTPUT
// "halo, saya thobie" akan berubah menjadi "halo, saya raihan" setelah button ganti di klik
```

### cara penggunaan useEffect Hooks
- biasa digunakan saat membuat suatu call API
- useEffect merupakan hooks yang bisa digunakan untuk menggunakan lifecycle pada functional components dengan mudah
- useEffect bisa dimasukkan sebelum melakukan render, useEffect biasanya ditempatkan dibawa useState
- setiap code yang kita tuliskan dalam useEffect akan dijalankan setiap component baru di mount (componentDidMount), terjadi perubahan (componentDidUpdate), dan pada saat meninggalkan component (componentWillUnmount)

step penggunaan useEffect
1. import { useEffect } from 'react' ;
2. penggunaan useEffect sebelum render
```js
useEffect(() => {
    console.log("terjadi perubahan")
}, [nama])
```

Full code
```js
import { useEffect } from 'react' ;

function App() {
    const [nama, setNama] = useState(true);
    const [changed, setChanged] = useState(0);

    useEffect(() => {
        console.log("ada perubahan");
        setChanged(changed + 1);
    }, [nama])

    return(
        <>
            <p>perubahan : {changed}</p>
            <button onClick={ () => setNama('raihan')}> ganti </button>
    )
};
```

## React Router 6
- Routing merupakan proses pemetaan antara sebuah URL ke dalam sebuah halaman yang terdapat konten sesuai dengan URL yang dituju.
- tambahkan library `react-router-dom` dan `react-router`.
    >npm install react-router-dom react-router
- lakukan import `BrowserRouter`, `Route`, dan `Routes`.
    - `BrowserRouter` berfungsi sebagai router yang melakukan API history, sehingga dapat menggunakan *location* untuk mengsingkronasi UI dengan URL.
    - `Route` digunakan untuk merender UI saat path cocok dengan URL saat ini. di dalam component tersebut bisa ditambahkan `exact` yang bertugas untuk memastikan Route hanya merender component yang memiliki path dan location.pathname yang cocok.
    - `Routes` digunakan untuk membungkus `Route` yang mana hanya akan merender Route saat path nya cocok dengan URL.

terdapat 2 jenis Router yang dapat kita gunakan, yaitu
- HashRouter
- BrowserRouter

perbedaan diantara 2 jenis Router tersebut
- HashRouter digunakan apabila membuat web yang static tidak ada server untuk me-render data yang dinamis.
- BrowserRouter digunakan sebaliknya, jika membuat web yang menggunakan data dinamis dengan server Back-End maka menggunakan `BrowserRouter`.

Seperti yang kita ketahui, React menggunakan method ReactDom.render() untuk me-render root component kita ke dalam DOM, kode tersebut terdapat di index.js. Yang harus kita lakukan untuk memasang router adalah dengan me-wrap root component kita dengan BrowserRouter seperti berikut:
```js
import { render } from "react-dom";
import { BrowserRouter } from "react-router-dom";
import App from "./App";

const rootElement = document.getElementById("root");
render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  rootElement
);
```

Dynamic Routes

`src/App.js`, import `LINK`
```js
import { Link } from "react-router-dom";

export default function App() {
  return (
    <div>
      <h1>Bookkeeper</h1>
      <nav>
        <Link to="/ListBook">ListBook</Link> |{" "}
        <Link to="/Books/:id">Books</Link>
      </nav>
    </div>
  );
}
```

Add Routes
`src/routes/Books.jsx`
```js
import { useParams } from "react-router-dom"

export function Book() {
  const { id } = useParams()

  return (
    <h1>Book {id}</h1>
  );
}
```

`src/routes/ListBook.jsx`
```js
export default function ListBook() {
  return (
    <>
      <h2>ListBook</h2>
    </>
  );
}
```

React Router cara merender aplikasi kita di URL yang berbeda dengan membuat "Route Config" pertama kita di dalam `main.jsx` atau `index.js`.

`src/main.jsx`
```js
import { render } from "react-dom";
import { BrowserRouter, Routes, Route } from "react-router-dom";
import App from "./App";
import BookList from "./routes/BookList";
import Books from "./routes/Books";

const rootElement = document.getElementById("root");
render(
  <BrowserRouter>
    <Routes>
      <Route path="/" element={<App />} />
        <Route path="BookList" element={<BookList />} />
        <Route path="Books/:id" element={<Books />} />
    </Routes>
  </BrowserRouter>,
  rootElement
);
```
Perhatikan di path "/" itu merender `<App>`. Pada "/Books" itu membuat `<Book />`.

Ketika rute memiliki Children route, ia melakukan dua hal:

1. menyarangkan(nested) URL ("/" + "BookList" dan "/" + "Books")
2. Ini akan menyarangkan komponen UI untuk tata letak bersama ketika children route yang cocok:
Namun, sebelum (poin 2) berfungsi, kita perlu merender Outlet di rute "parent" App.jsx.

`src/App.jsx`
```js
import { Outlet, Link } from "react-router-dom";

export default function App() {
  return (
    <>
      <h1>Bookkeeper</h1>
      <nav>
        <Link to="/Books/:id">Books</Link> |{" "}
        <Link to="/BookList">BookList</Link>
      </nav>
      <Outlet />
    </>
  );
}
```

Nested Routes
```js
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/books">
    <Route index element={<BookList />} />
    <Route path=":id" element={<Book />} />
    <Route path="new" element={<NewBook />} />
  </Route>
  <Route path="*" element={<NotFound />} />
</Routes>
```
## State Management - Redux
- **Redux** dikenal sebagai library untuk untuk memonitor keadaan state kita saat ini/menampilkan data dan mengelola data.
- Redux dapat diibaratkan sebagai database pada sisi front-end.
- kita dapat melakukan operasi database, seperti :
    - query
    - filter
    - insert
    - delete
- redux tidak menyebutnya dengan database melainkan *Store* dan hanya ada **satu store** dalam satu aplikasi yang disebut *Single Source of Truth*.
- dengan menggunakan redux kita bisa membuat solusi global state.
- pusing-pusing mengelola state per komponen, state dari setiap komponen di pindahkan ke global state yang disebut *store*.
- Store ini akan terhubung dengan komponen. Selanjutnya kita hanya perlu berurusan dengan storenya si redux. Redux yang akan mengurus komunikasi komponent dan perubahan UI.

![contoh-redux](img/redux.jpeg)

- gunakan redux jika
    - Banyak data yang berubah dari waktu ke waktu
    - Pengelolaan state harus dilakukan di satu tempat (Single source of truth)
    - Mengelola state di top-level component sudah tidak lagi relevan

### Konsep Dasar
ada 4 istilah yang digunakan dalam Redux yang wajib untuk diketahui.

- Actions
Sebuah JavaScript Object mewakili apa yang terjadi di dalam aplikasi.

- Reducers
Function yang menerima object state dan object action, bertugas menentukan bagaimana suatu state diubah. Output reducer adalah state baru.

>Syntax: (state, action) => newState

- Store
Tempat dimana global state disimpan.

- Dispatch
Satu-satunya cara untuk mengubah state di dalam store adalah dengan memanggil method bernama dispatch yang berisi object action, kemudian Redux akan mengeksekusi reducer yang sesuai.

- Selector
Function yang digunakan untuk mendapatkan data dari state yang ada di dalam store.

### 3 Konsep Dasar
- Single Source of Truth
Redux menggunakan store sebagai single source of truth, dimana semua global state disimpan dalam bentuk object di dalam store.

- State is Read Only
Untuk menghindari data diubah tanpa sengaja atau terhapus, state hanya bisa diubah dengan cara dispatch suatu action.

- Changes are Made with Pure Reducer Function
State diubah menggunakan pure function (Reducer), yaitu function menerima state sebelumnya dan sebuah action, kemudian menghasilkan state baru tanpa menghapus state sebelumnya.

## State Management - Async Actions With Redux Thunk and Middleware
#### Data Flow in Redux
4 step utama untuk Data Lifecycle di Redux
- An event inside your app triggers a call to `store.dispatch(actionCreator(payload))`.
- The Redux store calls the root reducer with the current state and the action.
- The root reducer combines the output of multiple reducers into a single state tree.

### Async Actions with Middleware and Thunk

#### Redux Middleware and Side Effects
Earlier, we said that Redux reducers must never contain "side effects". A "side effect" is any change to state or behavior that can be seen outside of returning a value from a function. Some common kinds of side effects are things like:

- Logging a value to the console
- Saving a file
- Setting an async timer
- Making an AJAX HTTP request
- Modifying some state that exists outside of a function, or mutating arguments to a function
- Generating random numbers or unique random IDs (such as - Math.random() or Date.now())

**Redux middleware were designed to enable writing logic that has side effects.**

As we said in Part 4, a Redux middleware can do anything when it sees a dispatched action: log something, modify the action, delay the action, make an async call, and more. Also, since middleware form a pipeline around the real store.dispatch function, this also means that we could actually pass something that isn't a plain action object to dispatch, as long as a middleware intercepts that value and doesn't let it reach the reducers.

Middleware also have access to dispatch and getState. That means you could write some async logic in a middleware, and still have the ability to interact with the Redux store by dispatching actions.

#### Using Middleware to Enable Async Logic
Let's look at a couple examples of how middleware can enable us to write some kind of async logic that interacts with the Redux store.

One possibility is writing a middleware that looks for specific action types, and runs async logic when it sees those actions, like these examples:
```javascript
import { client } from '../api/client'

const delayedActionMiddleware = storeAPI => next => action => {
  if (action.type === 'todos/todoAdded') {
    setTimeout(() => {
      // Delay this action by one second
      next(action)
    }, 1000)
    return
  }

  return next(action)
}

const fetchTodosMiddleware = storeAPI => next => action => {
  if (action.type === 'todos/fetchTodos') {
    // Make an API call to fetch todos from the server
    client.get('todos').then(todos => {
      // Dispatch an action with the todos we received
      storeAPI.dispatch({ type: 'todos/todosLoaded', payload: todos })
    })
  }

  return next(action)
}
```

#### Redux Async Data Flow
So how do middleware and async logic affect the overall data flow of a Redux app?

Just like with a normal action, we first need to handle a user event in the application, such as a click on a button. Then, we call dispatch(), and pass in something, whether it be a plain action object, a function, or some other value that a middleware can look for.

Once that dispatched value reaches a middleware, it can make an async call, and then dispatch a real action object when the async call completes.

#### Redux Async Data Flow
As it turns out, Redux already has an official version of that "async function middleware", called the Redux "Thunk" middleware. The thunk middleware allows us to write functions that get dispatch and getState as arguments. The thunk functions can have any async logic we want inside, and that logic can dispatch actions and read the store state as needed.
![video](https://d33wubrfki0l68.cloudfront.net/08d01ed85246d3ece01963408572f3f6dfb49d41/4bc12/assets/images/reduxasyncdataflowdiagram-d97ff38a0f4da0f327163170ccc13e80.gif)

#### Configuring the Store
-  npm install redux-thunk
- Once it's installed, we can update the Redux store in our todo app to use that middleware:
```javascript
import { createStore, applyMiddleware } from 'redux'
import thunkMiddleware from 'redux-thunk'
import { composeWithDevTools } from 'redux-devtools-extension'
import rootReducer from './reducer'

const composedEnhancer = composeWithDevTools(applyMiddleware(thunkMiddleware))

// The store now has the ability to accept thunk functions in `dispatch`
const store = createStore(rootReducer, composedEnhancer)
export default store
```

- Right now our todo entries can only exist in the client's browser. We need a way to load a list of todos from the server when the app starts up.
```javascript
import { client } from '../../api/client'

const initialState = []

export default function todosReducer(state = initialState, action) {
  // omit reducer logic
}

// Thunk function
export async function fetchTodos(dispatch, getState) {
  const response = await client.get('/fakeApi/todos')
  dispatch({ type: 'todos/todosLoaded', payload: response.todos })
}
```

We only want to make this API call once, when the application loads for the first time. There's a few places we could put this:

- In the <App> component, in a useEffect hook
- In the <TodoList> component, in a useEffect hook
- In the index.js file directly, right after we import the store

```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import { Provider } from 'react-redux'
import './index.css'
import App from './App'

import './api/server'

import store from './store'
import { fetchTodos } from './features/todos/todosSlice'

store.dispatch(fetchTodos)

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
)
```
![image](https://d33wubrfki0l68.cloudfront.net/c96c315f5e60b87fa093e49d8d30a5a1fb66c580/9fd52/assets/images/devtools-todosloaded-action-0f8f36d9f37404a85e5cbc20bd30d9e4.png)

- The last thing we need to change here is updating our todos reducer. When we make a POST request to /fakeApi/todos, the server will return a completely new todo object (including a new ID value). That means our reducer doesn't have to calculate a new ID, or fill out the other fields - it only needs to create a new state array that includes the new todo item:
```javascript
const initialState = []

export default function todosReducer(state = initialState, action) {
  switch (action.type) {
    case 'todos/todoAdded': {
      // Return a new todos state array with the new todo item at the end
      return [...state, action.payload]
    }
    // omit other cases
    default:
      return state
  }
}
```
![image](https://d33wubrfki0l68.cloudfront.net/7b06b4966e3cfc821fed6c95c21ba0d4c4013b0b/e9fb9/assets/images/devtools-async-todoadded-diff-5f18f236969280dec2dad1a6d4d74b2e.png)