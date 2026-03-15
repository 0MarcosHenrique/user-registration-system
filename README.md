# 🎨 User Registration System (Frontend)

![React](https://img.shields.io/badge/React-19.x-blue)
![Vite](https://img.shields.io/badge/Vite-7.x-purple)
![Axios](https://img.shields.io/badge/Axios-1.x-green)
![License](https://img.shields.io/badge/License-MIT-yellow)

The glorious frontend for the User Registration API. This is where the magic happens – a clean, functional interface built with React and Vite that brings the backend data to life.

> "The frontend that survived the backend saga." 😎

---

## 📖 The Epic Continues: The Frontend Chapter

If the backend was a telenovela, the frontend is the **high-budget spin-off**! After conquering Prisma, MongoDB, and a thousand bugs on the server side, it was time to give the users (that's you) a way to actually interact with the data.

This interface was born to consume the `0MarcosHenrique/API` and provide a complete, real-world registration experience.

---

## ✨ Features

- ✅ **Complete Registration Form** with controlled inputs
- ✅ **Real-time User Listing** – see users appear instantly after creation
- ✅ **One-click Deletion** with a trash icon (satisfying, isn't it?)
- ✅ **Seamless API Integration** using Axios
- ✅ **Clean, Dark-themed UI** with CSS
- ✅ **`useRef` for Uncontrolled Inputs** (a classic React approach)
- ✅ **`useEffect` Magic** – automatically fetches users on page load

---

## 🛠️ Tech Stack

| Technology | Version | Purpose |
|------------|---------|---------|
| React | 19.x | UI Library |
| Vite | 7.x | Build Tool & Dev Server |
| Axios | 1.x | HTTP Client for API calls |
| CSS | - | Styling (no frameworks, pure skill) |

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** (v18 or higher)
- **npm** or **yarn**
- The **backend API** must be running on `http://localhost:3000` (the `0MarcosHenrique/API` project)

### Installation & Setup

1.  **Clone the repository**
    ```bash
    git clone https://github.com/0MarcosHenrique/user-registration-system.git
    cd user-registration-system
Install dependencies

bash
npm install
Run the development server

bash
npm run dev
Open your browser
Navigate to http://localhost:5173 (or the port Vite provides). Make sure your backend API is running on port 3000!

### 🧠 How It Works: The Main Component (Home/index.jsx)
This is the brain of the operation. Let's break down the magic:

State, Refs, and Side Effects<br>
jsx<br>
const [users, setUsers] = useState([]);        // Holds the list of users<br>

// Refs to grab input values directly from the DOM (uncontrolled component style)<br>
const inputName = useRef();<br>
const inputAge = useRef();<br>
const inputEmail = useRef();<br>
const inputPassword = useRef();<br>

// Fetches users from the backend when the component mounts
useEffect(() => {<br>
    getUsers();<br>
}, []);<br>
Talking to the API (via services/api.js)<br>
All API calls are centralized in src/services/api.js:<br>

javascript<br>
import axios from 'axios';<br>

const api = axios.create({<br>
    baseURL: 'http://localhost:3000', // Your backend API URL<br>
});<br>
export default api;
The Core Functions

### getUsers(): Fetches the user list and updates the state.<br>

javascript<br>
async function getUsers() {<br>
    const usersFromApi = await api.get('/users');<br>
    setUsers(usersFromApi.data);<br>
<br>}

---

### createUsers(): Sends new user data to the backend, then refreshes the list.

javascript<br>
async function createUsers() {<br>
   await api.post('/users', {<br>
        name: inputName.current.value,<br>
        age: inputAge.current.value,<br>
        email: inputEmail.current.value,<br>
        password: inputPassword.current.value,<br>
    });
    getUsers(); // Refresh the list
    // Optionally, clear the form here
}

---

### deleteUsers(id): Deletes a user and refreshes the list. So satisfying.

javascript<br>
async function deleteUsers(id) {<br>
    await api.delete(`/users/${id}`);<br>
    getUsers();<br>
<br>}

The Render: Form + User Cards
The component renders a form to add new users and a list of cards displaying existing users, each with a delete button.

---

### 📁 Project Structure

user-registration-system/<br>
├── public/            &nbsp;&nbsp;&nbsp;&nbsp;     # Static assets (favicon, etc.)<br>
├── src/<br>
│   ├── assets/<br>
│   │   └── trash.svg       &nbsp;&nbsp;&nbsp;&nbsp;# The iconic delete icon<br>
│   ├── pages/<br>
│   │   └── Home/<br>
│   │       ├── index.jsx   &nbsp;&nbsp;&nbsp;&nbsp;# The main component (all the logic)<br>
│   │       └── style.css   &nbsp;&nbsp;&nbsp;&nbsp;# Styles for the Home page<br>
│   ├── services/<br>
│   │   └── api.js          &nbsp;&nbsp;&nbsp;&nbsp;# Axios instance configured for the backend<br>
│   ├── index.css          &nbsp;&nbsp;&nbsp;&nbsp; # Global styles<br>
│   └── main.jsx            &nbsp;&nbsp;&nbsp;&nbsp;# Application entry point<br>
├── .gitignore<br>
├── eslint.config.js        &nbsp;&nbsp;&nbsp;&nbsp;# Linting rules<br>
├── index.html              &nbsp;&nbsp;&nbsp;&nbsp;# Main HTML file<br>
├── package.json           &nbsp;&nbsp;&nbsp;&nbsp;# Dependencies and scripts<br>
├── vite.config.js         &nbsp;&nbsp;&nbsp;&nbsp; # Vite configuration<br>
└── README.md              &nbsp;&nbsp;&nbsp;&nbsp; # You are here!<br>

---
### 🧪 Testing the Full Integration
Start the Backend:

bash
git clone https://github.com/0MarcosHenrique/API.git
cd API
npm install
# Set up your .env file with DATABASE_URL
npx prisma db push
node server.js
(API should be running on http://localhost:3000)

Start the Frontend:

bash
---
### In a new terminal, from the frontend project folder
npm run dev
Use the App:

Add a new user via the form.

Watch the card appear instantly in the list below.

Click the trash icon to delete a user.

Check your MongoDB via Prisma Studio (npx prisma studio in the backend folder) to confirm the changes.

🐛 Common Issues & Solutions (From Our Saga)
"Failed to load resource: net::ERR_CONNECTION_REFUSED"
Problem: Frontend can't reach the backend.

Solution: Ensure the backend server is running on http://localhost:3000. Check the baseURL in src/services/api.js.

CORS Error in Console
Problem: Backend isn't configured to accept requests from the frontend's origin (http://localhost:5173).

Solution: Make sure your backend (server.js) has the CORS middleware enabled:

javascript
import cors from 'cors';
app.use(cors()); // This allows all origins. Good for development.
New User Doesn't Appear in the List
Problem: The list isn't refreshing after creation.

Solution: The createUsers() function correctly calls getUsers() after the POST request. Check the browser's network tab to see if the POST and subsequent GET requests are successful.

The trash.svg Icon is Missing
Problem: Broken image icon.

Solution: Ensure the import path in Home/index.jsx is correct: import Trash from '../../assets/trash.svg';. The path is relative to the component file.
---
### 🗺️ Future Features (The Sequel)
Edit User Functionality: Add an "edit" button to each card.

Form Validation & Feedback: Show inline errors and success toasts.

Loading States: Disable buttons and show spinners during API calls.

Better Error Handling: Display friendly messages for duplicate emails, etc.

Search/Filter: Add an input to filter the displayed users.

Deploy: Host the frontend on Vercel or Netlify, and the backend on Render/Railway.
---
### 🧑‍💻 Author
Marcos Henrique (0MarcosHenrique)
Built with ☕, 🎧, and the determination to see "what is this???" turn into "It works! 🚀".
---
### 🎭 Special Thanks
To the Backend API, for existing and providing the data.

To React Hooks (useState, useEffect, useRef) for making state manageable.

To Axios, for being the reliable messenger.

To the Trash SVG, for making deletion so satisfying.

And to You, for reading this far and appreciating the journey.
---
### 📄 License
This project is licensed under the MIT License.
---
### 📸 Preview
(Imagine a beautiful screenshot of your app here)
---
### 🏁 The Finale (For Now)<br>
🎬 SEASON 1: The Backend Nightmare<br>
🎬 SEASON 2: The Frontend Awakens<br>
🎬 SEASON 3: A New Hope (Integration)<br>
🎬 SEASON 4: Return of the User (List)<br>
🎬 FINALE: The Full Stack Dream<br>
---
THE END...? (Probably not, stay tuned for the "Login with JWT" special episode) 🔥
