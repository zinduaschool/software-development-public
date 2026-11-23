# Firebase with React

[← Day 5 overview](README.md)

1. [What is Firebase?](#1-what-is-firebase)
2. [Set up a Firebase project](#2-set-up-a-firebase-project)
3. [Connect React to Firebase](#3-connect-react-to-firebase)
4. [Example 1: Simple authentication](#4-example-1-simple-authentication)
5. [Example 2: School portal with roles](#5-example-2-school-portal-with-roles)
6. [When something breaks](#6-when-something-breaks)

## 1. What is Firebase?

Firebase is Google's **Backend-as-a-Service (BaaS)**. It gives you ready-made backend services, like logins and a database, that you call straight from your React code. You don't build or run your own server.

- **React** builds what users see and click.
- **Firebase** handles the backend: who's logged in, the data, and files.
- They connect through the **Firebase JavaScript SDK**, an npm package called `firebase`.

The free **Spark** plan is enough for this lesson. Some products need the pay-as-you-go **Blaze** plan, which still has a free monthly allowance.

### The main products

| Product | What it does | Plan |
|---|---|---|
| Authentication | Sign-up, log in and log out with email/password, Google and other providers | Spark |
| Cloud Firestore | A NoSQL database. Apps can listen to data and update the moment it changes | Spark |
| Hosting | Serves your built React app from a global CDN | Spark |
| Cloud Functions | Runs your server-side code when something happens, without managing a server | Blaze |
| Cloud Storage | Stores user files like images and videos | Blaze |

This lesson uses **Authentication** and **Cloud Firestore**.

### How Firestore stores data

Firestore keeps data in **collections** of **documents**. A document is like a JavaScript object with fields. Every document has an ID.

```text
users                      <- collection
└── abc123                 <- document (ID = the user's UID)
    └── role: "admin"      <- field

courses                    <- collection
└── 7fK2xQ...              <- document (ID made by Firestore)
    ├── name: "React Basics"
    └── description: "Components, props and state"
```

You don't create collections by hand. A collection appears the first time you add a document to it.

## 2. Set up a Firebase project

Do this once for each app.

1. Go to the [Firebase console](https://console.firebase.google.com) and sign in with your Google account.
2. Create a new Firebase project and give it a name. You can skip Gemini and Google Analytics for this lesson.
3. On the project overview page, click the **Web** icon (`</>`), enter an app nickname and click **Register app**.
4. Keep the `firebaseConfig` values that appear. You'll need them in the next section. You can find them again any time under **Project settings → General → Your apps**.
5. Turn on email login: go to **Security → Authentication**, click **Get started** if you see it, open the **Sign-in method** tab, enable **Email/Password** and click **Save**.

## 3. Connect React to Firebase

Each example installs the SDK (`npm install firebase`) in its first step. Then create a file called `.env.local` in the project root (next to `package.json`) and paste in your config values:

```bash
# .env.local
VITE_FIREBASE_API_KEY=your-api-key
VITE_FIREBASE_AUTH_DOMAIN=your-project.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your-project
VITE_FIREBASE_STORAGE_BUCKET=your-project.firebasestorage.app
VITE_FIREBASE_MESSAGING_SENDER_ID=123456789
VITE_FIREBASE_APP_ID=1:123456789:web:abc123
```

Then create `src/firebase.js`. Every other file imports Firebase from here:

```js
// src/firebase.js
import { initializeApp } from 'firebase/app'
import { getAuth } from 'firebase/auth'

const firebaseConfig = {
  apiKey: import.meta.env.VITE_FIREBASE_API_KEY,
  authDomain: import.meta.env.VITE_FIREBASE_AUTH_DOMAIN,
  projectId: import.meta.env.VITE_FIREBASE_PROJECT_ID,
  storageBucket: import.meta.env.VITE_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: import.meta.env.VITE_FIREBASE_MESSAGING_SENDER_ID,
  appId: import.meta.env.VITE_FIREBASE_APP_ID,
}

const app = initializeApp(firebaseConfig)
export const auth = getAuth(app)
```

- `initializeApp` connects your app to your Firebase project. Call it once, before using any other Firebase feature.
- `getAuth(app)` gives you the `auth` object. Every auth function needs it.
- Vite only exposes variables that start with `VITE_`, and you read them with `import.meta.env`. Restart `npm run dev` after you change `.env.local`.
- `.env.local` is already in Vite's `.gitignore`, so it won't be pushed to GitHub. The config isn't a password (your security rules protect your data), but this keeps keys out of public repos and makes it easy to switch projects. When you deploy, add the same variables in your hosting dashboard.

## 4. Example 1: Simple authentication

Sign up, log in and log out with email and password, and stay logged in after a refresh.

### Step 1: Create the app

```bash
npm create vite@latest simple-auth -- --template react
cd simple-auth
npm install
npm install firebase
```

Set up a Firebase project ([section 2](#2-set-up-a-firebase-project)), then add `.env.local` and `src/firebase.js` ([section 3](#3-connect-react-to-firebase)).

### Step 2: Clean up the starter

Delete `src/App.css` and the `src/assets` folder (the new `App.jsx` in Step 3 doesn't use them), then replace `src/index.css` with a few simple styles:

```css
/* src/index.css */
:root {
  font-family: system-ui, sans-serif;
  line-height: 1.5;
  color-scheme: light dark;
}

body {
  margin: 0;
}

#root {
  max-width: 480px;
  margin: 3rem auto;
  padding: 0 1rem;
}

input,
select,
button {
  display: block;
  width: 100%;
  margin-bottom: 0.75rem;
  padding: 0.6rem;
  font: inherit;
  box-sizing: border-box;
}

button {
  cursor: pointer;
}

ul {
  padding: 0;
  list-style: none;
}

li {
  padding: 0.75rem 0;
  border-bottom: 1px solid #8884;
}

li button {
  display: inline-block;
  width: auto;
  margin: 0.5rem 0.5rem 0 0;
}

.error {
  color: crimson;
}
```

### Step 3: The App component

```jsx
// src/App.jsx
import { useEffect, useState } from 'react'
import {
  createUserWithEmailAndPassword,
  onAuthStateChanged,
  signInWithEmailAndPassword,
  signOut,
} from 'firebase/auth'
import { auth } from './firebase'

function App() {
  const [user, setUser] = useState(null)
  const [loading, setLoading] = useState(true)
  const [email, setEmail] = useState('')
  const [password, setPassword] = useState('')
  const [error, setError] = useState('')

  useEffect(() => {
    const unsubscribe = onAuthStateChanged(auth, (currentUser) => {
      setUser(currentUser)
      setLoading(false)
    })
    return unsubscribe
  }, [])

  const signUp = async () => {
    setError('')
    try {
      await createUserWithEmailAndPassword(auth, email, password)
    } catch (err) {
      setError(err.message)
    }
  }

  const logIn = async () => {
    setError('')
    try {
      await signInWithEmailAndPassword(auth, email, password)
    } catch (err) {
      setError(err.message)
    }
  }

  if (loading) return <p>Loading...</p>

  if (user) {
    return (
      <main>
        <h1>Welcome, {user.email}</h1>
        <button onClick={() => signOut(auth)}>Log out</button>
      </main>
    )
  }

  return (
    <main>
      <h1>Simple Authentication</h1>
      <input
        type="email"
        placeholder="Email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      <input
        type="password"
        placeholder="Password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      />
      <button onClick={logIn}>Log in</button>
      <button onClick={signUp}>Sign up</button>
      {error && <p className="error">{error}</p>}
    </main>
  )
}

export default App
```

How it works:

- **`onAuthStateChanged(auth, callback)`** runs once when the page loads and again every time someone logs in or out. Firebase remembers the logged-in user between refreshes, and this listener is how your app finds out who that is. It returns an unsubscribe function, which `useEffect` calls on cleanup.
- **`loading`** stays `true` until Firebase has checked. Without it, a logged-in user would see the login form flash on every refresh.
- **`createUserWithEmailAndPassword`** creates the account and logs the user in. **`signInWithEmailAndPassword`** logs in an existing user. **`signOut`** logs them out.
- None of these handlers call `setUser`. The listener updates `user` for every case, so there's only one place that decides who's logged in.
- Firebase errors look like `Firebase: Error (auth/invalid-credential)`. Example 2 turns them into friendly messages.

### Step 4: Try it

```bash
npm run dev
```

Sign up, refresh the page (you stay logged in), log out, then log back in. Your new user shows up in the console under **Security → Authentication → Users**.

## 5. Example 2: School portal with roles

A portal with three roles:

| Role | Can do | How they get an account |
|---|---|---|
| Admin | Add, edit and delete courses | Created by hand in the Firebase console |
| Teacher | View courses | Registers in the app |
| Student | View courses | Registers in the app |

The course list updates live. When the admin adds a course, every open dashboard shows it straight away, with no refresh.

What you'll build:

```text
src/
├── components/
│   ├── AdminDashboard.jsx
│   ├── CourseList.jsx
│   ├── Login.jsx
│   ├── Register.jsx
│   ├── StudentDashboard.jsx
│   └── TeacherDashboard.jsx
├── App.jsx
├── firebase.js
├── index.css
└── main.jsx        <- leave as Vite made it
```

### Step 1: Create the app

```bash
npm create vite@latest school-portal -- --template react
cd school-portal
npm install
npm install firebase react-router
```

Clean up the starter the same way as Example 1: delete `src/App.css` and `src/assets`, and use the same `src/index.css`.

> [!NOTE]
> React Router 8 is imported from `react-router`. Older tutorials use `react-router-dom`, which no longer exists in v8. It also needs Node.js 22.22 or newer (check with `node -v`).

### Step 2: Set up Firebase

1. Create a Firebase project, register a web app and enable **Email/Password** ([section 2](#2-set-up-a-firebase-project)). Add `.env.local` ([section 3](#3-connect-react-to-firebase)).
2. Create the database: go to **Databases & Storage → Firestore** and click **Create database**. Choose **Standard edition**, keep the default database ID, pick the location closest to your users (you can't change it later), and choose **Production mode**.
3. Create an admin account:
   1. In **Security → Authentication → Users**, click **Add user** and enter an email and password, for example `admin@school.com`.
   2. Copy that user's **User UID** from the users list.
   3. In **Firestore → Data**, click **Start collection** and name it `users`. Paste the UID as the **Document ID**, then add a field named `role`, type **string**, value `admin`. Click **Save**.

### Step 3: Add security rules

Production mode blocks every read and write until you write rules. In **Firestore → Rules**, replace everything with this and click **Publish**:

```js
// firestore.rules
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {

    function signedIn() {
      return request.auth != null;
    }

    function isAdmin() {
      return signedIn()
        && get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'admin';
    }

    // users/{uid} holds one field: role
    match /users/{userId} {
      allow read: if signedIn() && request.auth.uid == userId;
      allow create: if signedIn()
        && request.auth.uid == userId
        && request.resource.data.keys().hasOnly(['role'])
        && request.resource.data.role in ['student', 'teacher'];
    }

    match /courses/{courseId} {
      allow read: if signedIn();
      allow write: if isAdmin();
    }
  }
}
```

- **`users`:** you can only read your own document. You can create it once, with the role `student` or `teacher`. Nobody can make themselves an admin from the app, and nobody can change their role later. The admin's document works because the console isn't bound by these rules.
- **`courses`:** anyone logged in can read them. Only admins can add, edit or delete.
- **`get(...)`** reads the user's role document, so the rules can check whether they're an admin.

> [!WARNING]
> Don't use **Test mode** for anything real. It lets anyone on the internet read and change your whole database for 30 days.

> [!NOTE]
> Anyone can register as a teacher here, to keep the example small. In a real school, an admin would approve teachers.

### Step 4: Firebase helpers

Replace `src/firebase.js` with this fuller version. It holds every Firebase call the app makes, so the components stay simple:

```js
// src/firebase.js
import { initializeApp } from 'firebase/app'
import {
  createUserWithEmailAndPassword,
  getAuth,
  signInWithEmailAndPassword,
  signOut,
} from 'firebase/auth'
import {
  addDoc,
  collection,
  deleteDoc,
  doc,
  getFirestore,
  onSnapshot,
  orderBy,
  query,
  setDoc,
  updateDoc,
} from 'firebase/firestore'

const firebaseConfig = {
  apiKey: import.meta.env.VITE_FIREBASE_API_KEY,
  authDomain: import.meta.env.VITE_FIREBASE_AUTH_DOMAIN,
  projectId: import.meta.env.VITE_FIREBASE_PROJECT_ID,
  storageBucket: import.meta.env.VITE_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: import.meta.env.VITE_FIREBASE_MESSAGING_SENDER_ID,
  appId: import.meta.env.VITE_FIREBASE_APP_ID,
}

const app = initializeApp(firebaseConfig)
export const auth = getAuth(app)
const db = getFirestore(app)

// ---------- Auth ----------

export const registerUser = (email, password) =>
  createUserWithEmailAndPassword(auth, email, password)

export const loginUser = (email, password) =>
  signInWithEmailAndPassword(auth, email, password)

export const logoutUser = () => signOut(auth)

const errorMessages = {
  'auth/email-already-in-use': 'An account with this email already exists.',
  'auth/invalid-email': 'Enter a valid email address.',
  'auth/weak-password': 'Password must be at least 6 characters.',
  'auth/invalid-credential': 'Wrong email or password.',
  'auth/too-many-requests': 'Too many attempts. Wait a few minutes and try again.',
}

export const getErrorMessage = (error) => errorMessages[error.code] ?? error.message

// ---------- Roles: one document per user at users/{uid} ----------

export const setUserRole = (uid, role) => setDoc(doc(db, 'users', uid), { role })

export const watchUserRole = (uid, callback) =>
  onSnapshot(
    doc(db, 'users', uid),
    (snapshot) => callback(snapshot.exists() ? snapshot.data().role : null),
    (error) => {
      console.error(error)
      callback(null)
    }
  )

// ---------- Courses ----------

const coursesRef = collection(db, 'courses')

export const watchCourses = (callback) =>
  onSnapshot(query(coursesRef, orderBy('name')), (snapshot) =>
    callback(snapshot.docs.map((course) => ({ id: course.id, ...course.data() })))
  )

export const addCourse = (course) => addDoc(coursesRef, course)

export const updateCourse = (id, course) => updateDoc(doc(db, 'courses', id), course)

export const deleteCourse = (id) => deleteDoc(doc(db, 'courses', id))
```

- **Auth helpers** wrap Firebase's functions so components can call `loginUser(email, password)` without importing `auth`. `auth` is still exported because `App.jsx` needs it to listen for logins.
- **`getErrorMessage`** turns Firebase error codes into messages users understand. Anything not in the list falls back to Firebase's own message.
- **`setUserRole`** writes `{ role }` to `users/{uid}`. `doc(db, 'users', uid)` points at one document, and `setDoc` creates it.
- **`watchUserRole`** and **`watchCourses`** use `onSnapshot`, which sends the current data straight away and then again every time it changes. Each returns an unsubscribe function.
- **`watchCourses`** sorts by name with `query(..., orderBy('name'))` and adds each document's ID to its data, because the ID isn't one of the fields.
- **`addDoc`** creates a course with an ID made by Firestore. **`updateDoc`** changes the fields you pass. **`deleteDoc`** removes the document.

### Step 5: Routing and auth in App

`App` decides who's logged in, what their role is, and which pages they're allowed to see:

```jsx
// src/App.jsx
import { useEffect, useState } from 'react'
import { BrowserRouter, Navigate, Route, Routes } from 'react-router'
import { onAuthStateChanged } from 'firebase/auth'
import { auth, logoutUser, watchUserRole } from './firebase'
import Login from './components/Login'
import Register from './components/Register'
import AdminDashboard from './components/AdminDashboard'
import TeacherDashboard from './components/TeacherDashboard'
import StudentDashboard from './components/StudentDashboard'

const ROLES = ['admin', 'teacher', 'student']

function App() {
  const [user, setUser] = useState(null)
  const [role, setRole] = useState(null)
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    let stopWatchingRole = () => {}

    const stopWatchingAuth = onAuthStateChanged(auth, (currentUser) => {
      stopWatchingRole()
      setUser(currentUser)

      if (!currentUser) {
        setRole(null)
        setLoading(false)
        return
      }

      stopWatchingRole = watchUserRole(currentUser.uid, (newRole) => {
        setRole(newRole)
        setLoading(false)
      })
    })

    return () => {
      stopWatchingRole()
      stopWatchingAuth()
    }
  }, [])

  if (loading) return <p>Loading...</p>

  // Logged in, but no valid role yet: just registered, or the role document is missing
  if (user && !ROLES.includes(role)) {
    return (
      <main>
        <p>Loading your account...</p>
        <button onClick={logoutUser}>Log out</button>
      </main>
    )
  }

  const home = `/${role}-dashboard`

  const onlyFor = (allowedRole, page) =>
    role === allowedRole ? page : <Navigate to="/login" replace />

  return (
    <BrowserRouter>
      <Routes>
        <Route path="/login" element={user ? <Navigate to={home} replace /> : <Login />} />
        <Route path="/register" element={user ? <Navigate to={home} replace /> : <Register />} />
        <Route path="/admin-dashboard" element={onlyFor('admin', <AdminDashboard />)} />
        <Route path="/teacher-dashboard" element={onlyFor('teacher', <TeacherDashboard />)} />
        <Route path="/student-dashboard" element={onlyFor('student', <StudentDashboard />)} />
        <Route path="*" element={<Navigate to="/login" replace />} />
      </Routes>
    </BrowserRouter>
  )
}

export default App
```

- **Two listeners.** `onAuthStateChanged` tells us who's logged in. For a logged-in user, `watchUserRole` then listens to their role document. When the user changes, the old role listener stops first.
- **Why listen to the role instead of reading it once?** Registering logs the user in *before* their role is saved. With a one-time read, App would see no role and send a new teacher to the wrong place. Listening means App updates the moment the role document is written.
- **`loading`** hides the routes until Firebase has checked, so nobody gets bounced to the login page on refresh.
- **"Loading your account..."** shows for a moment after logging in or registering. If it never goes away, that user has no role document, or the role isn't exactly `admin`, `teacher` or `student`. See [When something breaks](#6-when-something-breaks).
- **Routes:**
  - Logged-in users who visit `/login` or `/register` go to their own dashboard (`/admin-dashboard`, `/teacher-dashboard` or `/student-dashboard`).
  - `onlyFor('admin', <AdminDashboard />)` shows the page only to admins. Everyone else goes to `/login`, and from there logged-in users go to their own dashboard.
  - `*` catches every other URL.
  - `replace` swaps the current history entry, so the back button doesn't loop between redirects.

### Step 6: Log in and register

```jsx
// src/components/Login.jsx
import { useState } from 'react'
import { Link } from 'react-router'
import { getErrorMessage, loginUser } from '../firebase'

function Login() {
  const [email, setEmail] = useState('')
  const [password, setPassword] = useState('')
  const [error, setError] = useState('')
  const [submitting, setSubmitting] = useState(false)

  const handleSubmit = async (e) => {
    e.preventDefault()
    setError('')
    setSubmitting(true)

    try {
      await loginUser(email, password)
      // No navigate() needed: App sees the new user and opens their dashboard
    } catch (err) {
      setError(getErrorMessage(err))
      setSubmitting(false)
    }
  }

  return (
    <form onSubmit={handleSubmit}>
      <h1>Log in</h1>
      <input
        type="email"
        placeholder="Email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        required
      />
      <input
        type="password"
        placeholder="Password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
        required
      />
      <button type="submit" disabled={submitting}>
        {submitting ? 'Logging in...' : 'Log in'}
      </button>
      {error && <p className="error">{error}</p>}
      <p>
        No account yet? <Link to="/register">Register</Link>
      </p>
    </form>
  )
}

export default Login
```

```jsx
// src/components/Register.jsx
import { useState } from 'react'
import { Link } from 'react-router'
import { getErrorMessage, registerUser, setUserRole } from '../firebase'

function Register() {
  const [email, setEmail] = useState('')
  const [password, setPassword] = useState('')
  const [role, setRole] = useState('student')
  const [error, setError] = useState('')
  const [submitting, setSubmitting] = useState(false)

  const handleSubmit = async (e) => {
    e.preventDefault()
    setError('')
    setSubmitting(true)

    try {
      const { user } = await registerUser(email, password)
      await setUserRole(user.uid, role)
      // Firebase logs new users in straight away, so App opens their dashboard
    } catch (err) {
      setError(getErrorMessage(err))
      setSubmitting(false)
    }
  }

  return (
    <form onSubmit={handleSubmit}>
      <h1>Create an account</h1>
      <input
        type="email"
        placeholder="Email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        required
      />
      <input
        type="password"
        placeholder="Password (at least 6 characters)"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
        minLength={6}
        required
      />
      <select value={role} onChange={(e) => setRole(e.target.value)}>
        <option value="student">Student</option>
        <option value="teacher">Teacher</option>
      </select>
      <button type="submit" disabled={submitting}>
        {submitting ? 'Creating account...' : 'Register'}
      </button>
      {error && <p className="error">{error}</p>}
      <p>
        Already have an account? <Link to="/login">Log in</Link>
      </p>
    </form>
  )
}

export default Register
```

- Neither component calls `navigate()`. Once Firebase logs the user in, App sees it and opens the right dashboard.
- `required`, `type="email"` and `minLength={6}` let the browser check the form before anything is sent. Firebase checks again and returns an error if something's wrong.
- `submitting` disables the button while waiting, so a double click doesn't send the form twice.
- Register saves the role straight after creating the account. The role dropdown has no admin option, and the security rules block it anyway.

### Step 7: The course list

All three dashboards show courses, so the list is its own component:

```jsx
// src/components/CourseList.jsx
import { useEffect, useState } from 'react'
import { watchCourses } from '../firebase'

function CourseList({ onEdit, onDelete }) {
  const [courses, setCourses] = useState([])
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    const unsubscribe = watchCourses((data) => {
      setCourses(data)
      setLoading(false)
    })
    return unsubscribe
  }, [])

  if (loading) return <p>Loading courses...</p>
  if (courses.length === 0) return <p>No courses yet.</p>

  return (
    <ul>
      {courses.map((course) => (
        <li key={course.id}>
          <strong>{course.name}</strong> - {course.description}
          <div>
            {onEdit && <button onClick={() => onEdit(course)}>Edit</button>}
            {onDelete && <button onClick={() => onDelete(course.id)}>Delete</button>}
          </div>
        </li>
      ))}
    </ul>
  )
}

export default CourseList
```

- `watchCourses` keeps `courses` in sync with Firestore. Returning `unsubscribe` from `useEffect` stops listening when the component leaves the page.
- **Edit** and **Delete** only show when a parent passes `onEdit` and `onDelete`. The admin passes them; students and teachers don't.

### Step 8: Dashboards

```jsx
// src/components/AdminDashboard.jsx
import { useState } from 'react'
import CourseList from './CourseList'
import { addCourse, deleteCourse, logoutUser, updateCourse } from '../firebase'

function AdminDashboard() {
  const [name, setName] = useState('')
  const [description, setDescription] = useState('')
  const [editingId, setEditingId] = useState(null)
  const [error, setError] = useState('')

  const resetForm = () => {
    setName('')
    setDescription('')
    setEditingId(null)
  }

  const handleSubmit = async (e) => {
    e.preventDefault()
    setError('')
    const course = { name, description }

    try {
      if (editingId) {
        await updateCourse(editingId, course)
      } else {
        await addCourse(course)
      }
      resetForm()
    } catch (err) {
      setError(err.message)
    }
  }

  const handleEdit = (course) => {
    setName(course.name)
    setDescription(course.description)
    setEditingId(course.id)
  }

  const handleDelete = async (id) => {
    if (!window.confirm('Delete this course?')) return

    try {
      await deleteCourse(id)
      if (id === editingId) resetForm()
    } catch (err) {
      setError(err.message)
    }
  }

  return (
    <main>
      <h1>Admin Dashboard</h1>
      <button onClick={logoutUser}>Log out</button>

      <h2>{editingId ? 'Edit course' : 'Add a course'}</h2>
      <form onSubmit={handleSubmit}>
        <input
          placeholder="Course name"
          value={name}
          onChange={(e) => setName(e.target.value)}
          required
        />
        <input
          placeholder="Description"
          value={description}
          onChange={(e) => setDescription(e.target.value)}
          required
        />
        <button type="submit">{editingId ? 'Save changes' : 'Add course'}</button>
        {editingId && (
          <button type="button" onClick={resetForm}>
            Cancel
          </button>
        )}
      </form>
      {error && <p className="error">{error}</p>}

      <h2>Courses</h2>
      <CourseList onEdit={handleEdit} onDelete={handleDelete} />
    </main>
  )
}

export default AdminDashboard
```

- One form adds and edits. `editingId` is `null` when adding, and holds the course ID when editing.
- After a save or delete there's no refetching. `CourseList` is listening, so it updates by itself.
- Log out just calls `logoutUser`. App sees there's no user and shows the login page.

```jsx
// src/components/TeacherDashboard.jsx
import CourseList from './CourseList'
import { logoutUser } from '../firebase'

function TeacherDashboard() {
  return (
    <main>
      <h1>Teacher Dashboard</h1>
      <button onClick={logoutUser}>Log out</button>

      <h2>Courses</h2>
      <CourseList />
    </main>
  )
}

export default TeacherDashboard
```

```jsx
// src/components/StudentDashboard.jsx
import CourseList from './CourseList'
import { logoutUser } from '../firebase'

function StudentDashboard() {
  return (
    <main>
      <h1>Student Dashboard</h1>
      <button onClick={logoutUser}>Log out</button>

      <h2>Courses</h2>
      <CourseList />
    </main>
  )
}

export default StudentDashboard
```

### Step 9: Try it

```bash
npm run dev
```

1. Register as a student. You land on the student dashboard, with no courses yet.
2. Log out and log in as the admin. Add two courses, edit one, then delete one.
3. Open a second browser (or a private window), log in as the student, and add a course from the admin window. It appears for the student straight away.
4. Register a teacher and check you land on the teacher dashboard.
5. While logged in as the student, go to `/admin-dashboard`. You're sent back to the student dashboard.

## 6. When something breaks

| What happens | Why | Fix |
|---|---|---|
| `auth/invalid-api-key` as soon as the app loads | The config values aren't being read | Check `.env.local` is in the project root, every name starts with `VITE_`, then restart `npm run dev` |
| `auth/operation-not-allowed` or `auth/configuration-not-found` on sign-up | Email/Password login isn't turned on | **Security → Authentication → Sign-in method**, enable **Email/Password** |
| `Missing or insufficient permissions` | The rules block the request | Publish the rules from [Step 3](#step-3-add-security-rules), and check the user has a role document |
| Stuck on "Loading your account..." | The user has no document in `users`, or its `role` is misspelled. This usually happens with the admin | In **Firestore → Data**, the Document ID must be the user's UID and `role` must be `admin`, `teacher` or `student` (lowercase string) |
| Vite error about JSX in a `.js` file | Components were saved as `.js` | Vite needs `.jsx` for files that contain JSX |
| `Failed to resolve import "react-router-dom"` | Old tutorial code | Import from `react-router` |
