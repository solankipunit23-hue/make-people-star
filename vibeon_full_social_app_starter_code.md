# VibeOn — Full Social Media Website & App Starter

Tagline: **“Your Vibe. Your Space.”**

---

# 1. Project Overview

VibeOn is a modern Gen-Z social media platform with:

- Reels & photo posts
- Stories
- Messaging
- Privacy controls
- Dark mode
- Explore feed
- Creator profiles
- Real-time notifications
- Mobile-first UI

Tech Stack:

- React + Vite
- Tailwind CSS
- Firebase
- Framer Motion
- React Router
- Vercel Hosting

---

# 2. Create Project

Open terminal:

```bash
npm create vite@latest vibeon -- --template react
cd vibeon
npm install
```

Install packages:

```bash
npm install react-router-dom firebase framer-motion react-icons
```

Install Tailwind:

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

---

# 3. tailwind.config.js

```js
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    './index.html',
    './src/**/*.{js,ts,jsx,tsx}',
  ],
  theme: {
    extend: {
      colors: {
        primary: '#8B5CF6',
        secondary: '#06B6D4',
        dark: '#0F0F14'
      }
    },
  },
  plugins: [],
}
```

---

# 4. src/index.css

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

body {
  margin: 0;
  background: #0F0F14;
  color: white;
  font-family: Inter, sans-serif;
}
```

---

# 5. Folder Structure

```bash
src/
 ├── components/
 ├── pages/
 ├── firebase/
 ├── context/
 ├── App.jsx
 └── main.jsx
```

---

# 6. Firebase Setup

Create project:

https://firebase.google.com

Enable:

- Authentication
- Firestore
- Storage

---

# 7. firebase.js

Create:

src/firebase/firebase.js

```js
import { initializeApp } from 'firebase/app'
import { getAuth } from 'firebase/auth'
import { getFirestore } from 'firebase/firestore'
import { getStorage } from 'firebase/storage'

const firebaseConfig = {
  apiKey: 'YOUR_API_KEY',
  authDomain: 'YOUR_DOMAIN',
  projectId: 'YOUR_PROJECT_ID',
  storageBucket: 'YOUR_BUCKET',
  messagingSenderId: 'YOUR_SENDER_ID',
  appId: 'YOUR_APP_ID'
}

const app = initializeApp(firebaseConfig)

export const auth = getAuth(app)
export const db = getFirestore(app)
export const storage = getStorage(app)
```

---

# 8. App.jsx

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom'
import Home from './pages/Home'
import Login from './pages/Login'
import Signup from './pages/Signup'
import Feed from './pages/Feed'

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path='/' element={<Home />} />
        <Route path='/login' element={<Login />} />
        <Route path='/signup' element={<Signup />} />
        <Route path='/feed' element={<Feed />} />
      </Routes>
    </BrowserRouter>
  )
}

export default App
```

---

# 9. Home Page

Create:

src/pages/Home.jsx

```jsx
import { Link } from 'react-router-dom'
import { motion } from 'framer-motion'

export default function Home() {
  return (
    <div className='min-h-screen flex flex-col justify-center items-center text-center px-6 bg-dark'>
      <motion.h1
        initial={{ opacity: 0, y: -30 }}
        animate={{ opacity: 1, y: 0 }}
        className='text-6xl font-bold bg-gradient-to-r from-purple-500 to-cyan-400 bg-clip-text text-transparent'
      >
        VibeOn
      </motion.h1>

      <p className='mt-4 text-gray-300 text-xl'>
        Your Vibe. Your Space.
      </p>

      <div className='mt-8 flex gap-4'>
        <Link
          to='/signup'
          className='bg-primary px-6 py-3 rounded-full font-semibold'
        >
          Join Now
        </Link>

        <Link
          to='/login'
          className='border border-cyan-400 px-6 py-3 rounded-full'
        >
          Login
        </Link>
      </div>
    </div>
  )
}
```

---

# 10. Signup Page

Create:

src/pages/Signup.jsx

```jsx
import { useState } from 'react'
import { createUserWithEmailAndPassword } from 'firebase/auth'
import { auth } from '../firebase/firebase'
import { useNavigate } from 'react-router-dom'

export default function Signup() {
  const [email, setEmail] = useState('')
  const [password, setPassword] = useState('')

  const navigate = useNavigate()

  const handleSignup = async () => {
    try {
      await createUserWithEmailAndPassword(auth, email, password)
      navigate('/feed')
    } catch (err) {
      alert(err.message)
    }
  }

  return (
    <div className='min-h-screen flex justify-center items-center'>
      <div className='bg-zinc-900 p-8 rounded-2xl w-[350px]'>
        <h2 className='text-3xl font-bold mb-6'>Create Account</h2>

        <input
          type='email'
          placeholder='Email'
          className='w-full mb-4 p-3 rounded bg-zinc-800'
          onChange={(e) => setEmail(e.target.value)}
        />

        <input
          type='password'
          placeholder='Password'
          className='w-full mb-4 p-3 rounded bg-zinc-800'
          onChange={(e) => setPassword(e.target.value)}
        />

        <button
          onClick={handleSignup}
          className='w-full bg-primary py-3 rounded-xl'
        >
          Sign Up
        </button>
      </div>
    </div>
  )
}
```

---

# 11. Login Page

Create:

src/pages/Login.jsx

```jsx
import { useState } from 'react'
import { signInWithEmailAndPassword } from 'firebase/auth'
import { auth } from '../firebase/firebase'
import { useNavigate } from 'react-router-dom'

export default function Login() {
  const [email, setEmail] = useState('')
  const [password, setPassword] = useState('')

  const navigate = useNavigate()

  const handleLogin = async () => {
    try {
      await signInWithEmailAndPassword(auth, email, password)
      navigate('/feed')
    } catch (err) {
      alert(err.message)
    }
  }

  return (
    <div className='min-h-screen flex justify-center items-center'>
      <div className='bg-zinc-900 p-8 rounded-2xl w-[350px]'>
        <h2 className='text-3xl font-bold mb-6'>Welcome Back</h2>

        <input
          type='email'
          placeholder='Email'
          className='w-full mb-4 p-3 rounded bg-zinc-800'
          onChange={(e) => setEmail(e.target.value)}
        />

        <input
          type='password'
          placeholder='Password'
          className='w-full mb-4 p-3 rounded bg-zinc-800'
          onChange={(e) => setPassword(e.target.value)}
        />

        <button
          onClick={handleLogin}
          className='w-full bg-secondary py-3 rounded-xl'
        >
          Login
        </button>
      </div>
    </div>
  )
}
```

---

# 12. Feed Page

Create:

src/pages/Feed.jsx

```jsx
import { AiFillHeart } from 'react-icons/ai'

export default function Feed() {
  const posts = [
    {
      id: 1,
      user: 'vibe_user',
      image:
        'https://images.unsplash.com/photo-1503023345310-bd7c1de61c7d'
    }
  ]

  return (
    <div className='max-w-xl mx-auto py-10'>
      <h1 className='text-4xl font-bold mb-8'>Vibe Feed</h1>

      {posts.map((post) => (
        <div
          key={post.id}
          className='bg-zinc-900 rounded-2xl overflow-hidden mb-8'
        >
          <div className='p-4 font-semibold'>@{post.user}</div>

          <img
            src={post.image}
            className='w-full h-[450px] object-cover'
          />

          <div className='p-4 flex gap-4 text-2xl'>
            <AiFillHeart />
          </div>
        </div>
      ))}
    </div>
  )
}
```

---

# 13. Mobile App Conversion

Use:

https://www.pwabuilder.com

Convert website into:

- Android App
- Progressive Web App

---

# 14. Deploy Website

Push code to GitHub:

```bash
git init
git add .
git commit -m "VibeOn launch"
```

Create repo on GitHub.

Push:

```bash
git remote add origin YOUR_REPO_URL
git push -u origin main
```

Deploy using:

https://vercel.com

---

# 15. VibeOn Features Roadmap

## Phase 1

- Login/signup
- Feed
- Profiles
- Upload posts
- Likes/comments

## Phase 2

- Stories
- Reels
- Chat
- Notifications

## Phase 3

- AI captions
- Creator subscriptions
- Live streaming
- Voice rooms

---

# 16. Monetization

## Ads

Use:

https://adsense.google.com

## Creator Premium

Users pay for:

- badges
- themes
- exclusive rooms

## Brand Deals

Allow sponsored creator posts.

---

# 17. Privacy Features

## Important

Add:

- private accounts
- ghost mode
- screenshot alerts
- encrypted chats
- hidden online status

These features can make VibeOn unique.

---

# 18. Launch Marketing Strategy

## Create Social Pages

- Instagram
- YouTube Shorts
- TikTok
- X

Post:

- meme content
- creator edits
- viral trends
- app teasers

---

# 19. Viral Referral System

Invite 5 friends:

- unlock badge
- unlock themes
- premium profile border

---

# 20. Final Launch Checklist

✅ Domain name
✅ Firebase setup
✅ Vercel deploy
✅ Social media pages
✅ Invite first 100 users
✅ Upload first content
✅ Referral campaign

---

# Suggested Domain Names

- vibeon.app
- joinvibeon.com
- vibeon.social
- vibeonworld.com

---

# Final Advice

Do NOT try to compete with Instagram immediately.

Build:

- strong community
- clean UI
- privacy-first experience
- fast performance

Focus first on:

- students
- creators
- local communities
- Gen-Z users

Consistency + viral content + referrals = growth.

