# client packages

- [Tailwind and daisyui](#tailwind)
- [authentication](#authentication)
- [react router dom](#react-router-dom)
- [react toastify](#react-toastify)
- [react hot toast](#hot-toast)
- [axios](#axios)
- [react hook form](#react-hook-form)
- [react query](#react-query)
- [Icons](#Icons)
- [react day picker](#react-day-picker)
- [image upload](#image-upload)

# server packages

- [express server setup](#express-server)
- [mongodb get started](#mongodb-quick-start)

# video notes

- 76-7 (sendgrid custom email send)
- 67-5 (pagination)
- 63.5-4 (google map integration)
- 69-4 (useToken custom hook)
- 70.5 (jwt recap)
  \
   &nbsp;

---

## create react app

```bash
npx create-react-app my-app
#or
yarn create react-app my-app
```

## environment variable

- `.env.local` at root folder
- REACT_APP_credentialName=secret key

## [tailwind](https://tailwindcss.com/docs/guides/create-react-app) & [Daisy ui](https://daisyui.com/docs/install/)

```bash
yarn add -D tailwindcss postcss autoprefixer
```

```bash
yarn tailwindcss init -p
```

```bash
yarn add daisyui
```

- tailwind.config.js ->

_tip - if you face webpack error while editing config.js file, restart the client_

```js
module.exports = {
  content: ['./src/**/*.{js,jsx,ts,tsx}'],
  theme: {
    extend: {
      // tailwind themes here
    },
  },
  daisyui: {
    themes: false,
    // false means it won't apply daisy ui theme
  },
  plugins: [require('daisyui')],
}
```

- at index.css or App.css->

```css
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

@tailwind base;
@tailwind components;
@tailwind utilities;
```

\
 &nbsp;

---

## [authentication](https://github.com/CSFrequency/react-firebase-hooks/tree/master/auth)

### [firebase website](https://firebase.google.com/docs?gclid=EAIaIQobChMIx76hsMfu9wIVydeWCh3F8wpZEAAYASABEgJ_IvD_BwE&gclsrc=aw.ds)

```bash
yarn add firebase
```

```bash
yarn add react-firebase-hooks
```

_tip - don't forget to add providers on firebase website in authentication section before you start coding for the methods._

- ### [sign in with google](https://github.com/CSFrequency/react-firebase-hooks/tree/master/auth#social-login-example)

```js
// sign in with google
import { useSignInWithGoogle } from 'react-firebase-hooks/auth'
const [signInWithGoogle, gUser, gLoading, gError] = useSignInWithGoogle(auth)

function handler(arg) {
  signInWithGoogle()
}
```

- ### [create user with email](https://github.com/CSFrequency/react-firebase-hooks/tree/master/auth#usecreateuserwithemailandpassword)

```js
// create user with email with email verification
import { useCreateUserWithEmailAndPassword } from 'react-firebase-hooks/auth'
const [createUserWithEmailAndPassword, cUser, cLoading, cError] =
  useCreateUserWithEmailAndPassword(auth, { sendEmailVerification: true })

function handler(email,password) {
  createUserWithEmailAndPassword(email, password)
}
```

- ### [sign in with email](https://github.com/CSFrequency/react-firebase-hooks/tree/master/auth#usesigninwithemailandpassword)

```js
// login with email and password
import { useSignInWithEmailAndPassword } from 'react-firebase-hooks/auth'
const [signInWithEmailAndPassword, eUser, eLoading, eError] =
  useSignInWithEmailAndPassword(auth)

function handler(email, password) {
  signInWithEmailAndPassword(email, password)
}
```

- ### [sign in with github](https://github.com/CSFrequency/react-firebase-hooks/tree/master/auth#social-login-example)

```js
// sigin in with github
import { useSignInWithGithub } from 'react-firebase-hooks/auth'
const [signInWithGithub, gitUser, gitLoading, gitError] =
  useSignInWithGithub(auth)

function handler() {
  signInWithGithub()
}
```

- ### [update profile](https://github.com/CSFrequency/react-firebase-hooks/tree/master/auth#useupdateprofile)

```js
// update user profile
import { useUpdateProfile } from 'react-firebase-hooks/auth'
const [updateProfile, updating, updateError] = useUpdateProfile(auth)

async function handler(arg) {
  await updateProfile({ displayName, photoURL })
  alert('Updated profile')
}
```

- ### [send email verification](https://github.com/CSFrequency/react-firebase-hooks/tree/master/auth#usesendemailverification)

```js
// send email verification
import { useSendEmailVerification } from 'react-firebase-hooks/auth'
const [sendEmailVerification, sending, verifyError] =
  useSendEmailVerification(auth)

async function handler(arg) {
  await sendEmailVerification()
  alert('Sent email')
}
```

- ### [password reset](https://github.com/CSFrequency/react-firebase-hooks/tree/master/auth#usesendpasswordresetemail)

```js
// password reset email
import { useSendPasswordResetEmail } from 'react-firebase-hooks/auth'
const [sendPasswordResetEmail, sending, resetError] =
  useSendPasswordResetEmail(auth)

async function handler(arg) {
  await sendPasswordResetEmail(email)
  alert('Sent email')
}

// note: this hook does not give you confirmation if the email is exist or not...it says 'email sent' to any email you geive.ex- yadayada@mail.com
```

\
&nbsp;

---

## [react router dom](https://reactrouter.com/docs/en/v6/getting-started/tutorial)

```bash
yarn add react-router-dom
```

- ### [react router authentication(require auth)](https://stackblitz.com/github/remix-run/react-router/tree/main/examples/auth?file=src%2FApp.tsx)

```js
// require auth file
import { useAuthState } from 'react-firebase-hooks/auth'

function RequireAuth({ children }) {
  const [user, loading] = useAuthState(auth)
  const location = useLocation()

  if (laoding) {
    return <Spinner></Spinner>
  }
  if (!user) {
    return <Navigate to='/login' state={{ from: location }} replace />
  }

  return children
}
export default RequireAuth
```

```js
// authentication file
const navigate = useNavigate()
const location = useLocation()
const from = location.state?.from?.pathname || '/'

if (user) {
  navigate(from, { replace: true })
}
```

---

- ### [custom active link](https://stackblitz.com/github/remix-run/react-router/tree/main/examples/custom-link?file=src%2FApp.tsx)

```js
function CustomLink({ children, to, ...props }) {
  let resolved = useResolvedPath(to)
  let match = useMatch({ path: resolved.pathname, end: true })

  return (
    <div>
      <Link
        style={{ textDecoration: match ? 'underline' : 'none' }}
        to={to}
        {...props}
      >
        {children}
      </Link>
      // {match && ' (active)'}
    </div>
  )
}

export default CustomLink
```

\
&nbsp;

---

## [react toastify](https://fkhadra.github.io/react-toastify/installation)

```bash
# npm
npm install --save react-toastify

# yarn
yarn add react-toastify
```

```js
//inside app.js
import { ToastContainer, Slide } from 'react-toastify'
import 'react-toastify/dist/ReactToastify.css'

fun App() {
  <>
    <ToastContainer autoClose={3000} transition={Slide}/>
    other...compoents
  <>
}
```

```js
// usage
import { toast } from 'react-toastify'

toast.success('congrats!')
toast.error('invalid!')
toast.warning('warning')
toast.info('information')
toast('default toast')
```

- ### [see more toast options](https://fkhadra.github.io/react-toastify/introduction)
  \
  &nbsp;

---

## [hot toast](https://react-hot-toast.com/)

```bash
# npm
npm install react-hot-toast
# yarn
yarn add react-hot-toast
```

```js
// inside App.js
import { Toaster } from 'react-hot-toast';

fun App() {

  <div>
    <Toaster position="top-center" toastOptions={{duration: 3000,}}/>
  </div>
}
```

```js
// usage
import toast from 'react-hot-toast'
toast.success('Successfully created!')
toast.error('This is an error!')
toast.loading('Waiting...')
toast('Hello World')
```

- ### [see more toast options](https://react-hot-toast.com/)

\
&nbsp;

---

## [axios](https://axios-http.com/docs/example)

```bash
yarn add axios@0.25.0
```

```js
// axios get method
//----client
useEffect(() => {
  axios('http://localhost:5000/services?name=john', {
    headers: {
      authorization: `bearer secret key`,
    },
  }).then((res) => console.log(res))
}, [])

//-----server
app.get('/services', (req, res) => {
  const authorization = req.headers.authorization

  const queryParameter = req.query
  console.log(authorization)
  console.log(queryParameter)
  res.send(services)
})
```

```js
// Axios delete method
//-------client
const handleDelete = () => {
  axios
    .delete(`http://localhost:5000/services/${1}`, {
      data: {
        service: 'body from delete',
      },
      headers: {
        authorization: `bearer secret key from delete`,
      },
    })
    .then((res) => console.log(res))
}

//------------server

app.delete('/services/:id', (req, res) => {
  const query = req.params.id
  const authorization = req.headers.authorization
  const service = req.body
  console.log(authorization)
  console.log(query)
  console.log(service)
})
```

```js
// Axios post method
//--------client
const handlePost = () => {
  axios.post(
    `http://localhost:5000/services`,
    {
      service: 'body from post',
    },

    {
      headers: {
        authorization: `bearer secret key from post`,
      },
    }
  )
}

// --------server
app.post('/services', (req, res) => {
  const service = req.body
  const authorization = req.headers.authorization
  console.log(service)
  console.log(authorization)
})
```

```js
// axios put method
//------------client
const handlePut = () => {
  axios.put(
    `http://localhost:5000/services`,
    {
      service: 'body from put ',
    },
    {
      headers: {
        authorization: `bearer secret key from put`,
      },
    }
  )
}

//-------------server

app.put('/services', (req, res) => {
  const updatedService = req.body
  const authorization = req.headers.authorization
  console.log(updatedService)
  console.log(authorization)
})
```

```js
// axios patch method
const handlePatch = () => {
  axios.patch(
    `http://localhost:5000/services`,
    { service: 'body from patch' },
    {
      headers: {
        authorization: `bearer secret key from patch`,
      },
    }
  )
}

//-------------server
app.patch('/services', (req, res) => {
  const updatedService = req.body
  const authorization = req.headers.authorization
  console.log(updatedService)
  console.log(authorization)
})
```

\
&nbsp;

---

## [react hook form](https://react-hook-form.com/get-started)

```bash
yarn add react-hook-form
```

- ### [sample form](https://react-hook-form.com/get-started#Handleerrors)

- ### [form with basic validation](https://codesandbox.io/s/react-hook-form-basic-validation-qdyye)

\
&nbsp;

---

## [react query](https://react-query.tanstack.com/overview#enough-talk-show-me-some-code-already)

```bash
yarn add react-query
```

```js
// inside index.js
 import { QueryClient, QueryClientProvider, useQuery } from 'react-query'
const queryClient = new QueryClient()
<QueryClientProvider client={queryClient}>
  <App/>
</QueryClientProvider>
```

```js
// usage

//using axios
const { isLoading, error, data, refetch } = useQuery('repoData', () =>
  axios('https://api.github.com/repos/tannerlinsley/react-query').then((res) =>
    console.log(res)
  )
)

if (isLoading) return 'Loading...'

-------------------------------------------------------------------
//using fetch
const { isLoading, error, data,refetch } = useQuery('repoData', () =>
  fetch('https://api.github.com/repos/tannerlinsley/react-query').then((res) =>
    console.log(res.json())
  )
)
```

\
&nbsp;

---

## Icons

- ### [react icons](https://react-icons.github.io/react-icons/)

```bash
yarn add react-icons
```

- ### [hero icons](https://heroicons.com/)

```bash
yarn add @heroicons/react
```

- ### [font awesome](https://fontawesome.com/v5/docs/web/use-with/react)

```bash
yarn add @fortawesome/fontawesome-svg-core
yarn add @fortawesome/free-solid-svg-icons
yarn add @fortawesome/react-fontawesome
```

```js
// usage
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome'
import { faCoffee } from '@fortawesome/free-solid-svg-icons'

return (
  <button>
    <FontAwesomeIcon icon={faCoffee} />
  </button>
)
```

\
&nbsp;

---

```js
// usage
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome'
import { faCoffee } from '@fortawesome/free-solid-svg-icons'

return (
  <button>
    <FontAwesomeIcon icon={faCoffee} />
  </button>
)
```

\
&nbsp;

---

## [react day picker](https://react-day-picker.js.org/start)

```bash
yarn add react-day-picker date-fns
```

```js
import React from 'react'
import { DayPicker } from 'react-day-picker'

export default function App() {
  // disable all previous days
  const disabledDays = [
    { from: new Date(1600, 0, 1), to: new Date(Date.now() - 86400000) },
  ]

  return (
    <DayPicker
      defaultMonth={new Date()}
      disabled={disabledDays}
      mode='single'
    />
  )
}
```

\
&nbsp;

---

## image upload

```js
const imageAPI = 'api key from image hosting server'

async function handle() {
  const image = data.image[0]
  const formData = new FormData()
  formData.append('image', image)

  const url = `https://api.imgbb.com/1/upload?key=${imageAPI}`
    // using fetch
    fetch(url, {
      method: 'POST',
      body: formData,
    })

    //or using axios
    axios.post(url, formData ).then....`set hostedUrl to mongodb`
}
```

\
&nbsp;

---

- ### day picker css
  1. create a css file in src folder `daypicker.css`.
  2. import that into App.js `import './daypicker.css' `

```css
.rdp-day_selected:not([disabled]) {
  color: white;
  background-color: #0fcfec;
}

.rdp-day_selected:focus:not([disabled]) {
  background-color: #0fcfec;
  border: 2px solid rgb(138, 238, 252);
}

.rdp-day_selected:hover:not([disabled]) {
  background-color: #19d3ae;
  border: 2px solid rgb(152, 255, 215);
}
.rdp-button:active:not([disabled]) {
  border: 2px solid #19d3ae;
  background-color: #bafff1;
}
```

# server

## express server

```bash
# npm
npm install express cors mongodb dotenv jsonwebtoken

# yarn
yarn add express cors mongodb dotenv jsonwebtoken

# if use yarn you need to add *script* manually to package.json file
  "scripts": {
    "start": "node index.js",
    "start-dev": "nodemon index.js"
  },
```

- ### [express hello world api example](https://expressjs.com/en/starter/hello-world.html)
- ### [dotenv config call](https://github.com/motdotla/dotenv#usage)
- ### [cors usage](https://www.npmjs.com/package/cors#usage)


```js
// sample code
const express = require('express');
const cors = require('cors');
require('dotenv').config()

const app = express()
const port = process.env.PORT || 5000


app.get('/', (req, res) => {
  res.send('hello world')
})

app.listen(port, () => console.log('listening to port', port))

```

\

&nbsp;

---

## [mongodb quick start](https://www.mongodb.com/docs/drivers/node/current/quick-start/)

- ### [find doc](https://www.mongodb.com/docs/drivers/node/current/usage-examples/findOne/)

- ### [insert doc](https://www.mongodb.com/docs/drivers/node/current/usage-examples/insertOne/)

- ### [delete doc](https://www.mongodb.com/docs/drivers/node/current/usage-examples/deleteOne/)

- ### [update doc](https://www.mongodb.com/docs/drivers/node/current/usage-examples/updateOne/)

- ### [count doc](https://www.mongodb.com/docs/drivers/node/current/usage-examples/count/)

- ### [sort doc](https://www.mongodb.com/docs/drivers/node/current/fundamentals/crud/read-operations/sort/)

- ### [skip and limit doc](https://www.mongodb.com/docs/drivers/node/current/fundamentals/crud/read-operations/limit/#sample-documents)
