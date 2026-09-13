# 🚀 Personal Portfolio Website — Full Stack (MERN-style)

A complete, production-ready **full-stack personal portfolio website** built for an internship submission.

- **Frontend:** HTML5, CSS3, Vanilla JavaScript (fully responsive, no build step required)
- **Backend:** Node.js + Express.js (REST API)
- **Database:** MongoDB (via Mongoose)

---

## 📁 Folder Structure

```
portfolio-project/
├── backend/
│   ├── config/
│   │   └── db.js                # MongoDB connection logic
│   ├── models/
│   │   └── Contact.js           # Mongoose schema for contact form submissions
│   ├── routes/
│   │   └── contactRoutes.js     # /api/contact routes (POST, GET) with validation
│   ├── middleware/
│   │   └── errorHandler.js      # Centralized 404 + error handling
│   ├── .env.example             # Environment variable template
│   ├── .gitignore
│   ├── package.json
│   ├── server.js                # Express app entry point
│   └── vercel.json              # Config for deploying backend to Vercel
│
├── frontend/
│   ├── css/
│   │   └── style.css            # All styling (responsive, modern dark theme)
│   ├── js/
│   │   └── script.js            # All interactivity + API calls to backend
│   ├── assets/
│   │   └── README.txt           # Where to put your photo/resume
│   ├── index.html               # All 6 sections: Home, About, Skills, Projects, Education, Contact
│   └── netlify.toml             # Config for deploying frontend to Netlify
│
└── README.md                    # You are here
```

---

## ✨ Features

- Fully responsive, modern dark-themed UI (mobile, tablet, desktop)
- Animated typing effect, scroll-reveal animations, mobile hamburger nav
- **Home** — Hero intro with social links and CTA buttons
- **About Me** — Bio, quick info grid (education, role, location)
- **Skills** — Auto-rendered skill cards with animated progress bars
- **Projects** — Filterable project cards (name, description, tech stack, live + code links)
- **Education** — Vertical timeline of education & certifications
- **Contact Me** — Fully working contact form:
  - Client-side validation (name, email format, message length)
  - Sends data via `fetch()` to the Express backend
  - Backend validates again using `express-validator`
  - Data is saved into **MongoDB**
  - Success/error messages shown to the user
  - Basic rate-limiting to prevent spam
- Centralized error handling and clean REST API design on the backend

---

## 🧩 Tech Stack

| Layer      | Technology                                   |
|------------|-----------------------------------------------|
| Frontend   | HTML5, CSS3, JavaScript (ES6+), Font Awesome |
| Backend    | Node.js, Express.js                          |
| Database   | MongoDB, Mongoose                            |
| Validation | express-validator                            |
| Extras     | CORS, dotenv, morgan (logging), express-rate-limit |

---

## ⚙️ Prerequisites

Before running this project, make sure you have installed:

1. **Node.js** (v18 or later) — [Download here](https://nodejs.org/)
2. **MongoDB** — either:
   - **Local MongoDB** installed and running ([Download here](https://www.mongodb.com/try/download/community)), OR
   - A free **MongoDB Atlas** cluster (cloud) — [Sign up here](https://www.mongodb.com/cloud/atlas/register)
3. A code editor (VS Code recommended)
4. (Optional) The **VS Code "Live Server"** extension to run the frontend easily

---

## 🛠️ Step-by-Step: Run Locally

### 1. Download / Extract the project
Extract the ZIP file (or clone the repo) so you have the `portfolio-project` folder with `backend/` and `frontend/` inside it.

### 2. Set up the Backend

```bash
cd portfolio-project/backend
npm install
```

Create your environment file:

```bash
# Windows (Command Prompt)
copy .env.example .env

# macOS/Linux
cp .env.example .env
```

Open the new `.env` file and set your MongoDB connection string:

```env
PORT=5000
NODE_ENV=development
MONGO_URI=mongodb://127.0.0.1:27017/portfolioDB
CLIENT_ORIGIN=http://127.0.0.1:5500,http://localhost:5500,http://localhost:3000
```

> 💡 If you're using **MongoDB Atlas** instead of local MongoDB, replace `MONGO_URI` with your Atlas connection string, e.g.:
> `mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/portfolioDB`

Start the backend server:

```bash
npm run dev
```

(`npm run dev` uses `nodemon` for auto-restart on changes. Use `npm start` for a normal run.)

If everything is set up correctly, you'll see:

```
✅ MongoDB Connected: 127.0.0.1/portfolioDB
🚀 Server running in development mode on port 5000
```

Test it by visiting: `http://localhost:5000/api/health` — you should see a JSON success response.

### 3. Set up the Frontend

The frontend is plain HTML/CSS/JS, so **no npm install or build step is required.**

Open `frontend/index.html` in one of these ways:

**Option A — VS Code Live Server (recommended)**
1. Open the `frontend` folder in VS Code.
2. Right-click `index.html` → "Open with Live Server".
3. It will open at something like `http://127.0.0.1:5500`.

**Option B — Just double-click `index.html`**
This works too, but some browsers restrict `fetch()` calls from `file://` URLs. Live Server (or any local static server) is the safer option.

**Option C — Quick static server with Node**
```bash
cd portfolio-project/frontend
npx serve .
```

### 4. Connect Frontend to Backend

Open `frontend/js/script.js` and confirm the top of the file has:

```js
const API_BASE_URL = "http://localhost:5000/api";
```

This should match wherever your backend is running. Since the backend defaults to port `5000`, this works out of the box for local development.

### 5. Test the Contact Form

1. Fill out the Contact form on the site and click **Send Message**.
2. You should see a green success message.
3. Verify it was saved in MongoDB:
   - Using **MongoDB Compass**: connect to your `MONGO_URI`, open the `portfolioDB` database → `contacts` collection.
   - Or via API: visit `http://localhost:5000/api/contact` in your browser to see all saved submissions as JSON.

---

## 🔌 API Reference

| Method | Endpoint             | Description                              |
|--------|-----------------------|-------------------------------------------|
| GET    | `/api/health`         | Health check                              |
| POST   | `/api/contact`        | Submit a new contact form message         |
| GET    | `/api/contact`        | Get all contact submissions (demo/admin)  |
| GET    | `/api/contact/:id`    | Get a single contact submission by ID     |

**Example POST body for `/api/contact`:**
```json
{
  "name": "Jane Smith",
  "email": "jane@example.com",
  "subject": "Internship opportunity",
  "message": "Hi, I'd love to connect about a potential opportunity!"
}
```

**Success response (201):**
```json
{
  "success": true,
  "message": "Thank you! Your message has been received successfully.",
  "data": {
    "id": "665f1c2e4b1a2c001f9e4a1b",
    "name": "Jane Smith",
    "email": "jane@example.com",
    "subject": "Internship opportunity",
    "createdAt": "2026-09-13T10:15:00.000Z"
  }
}
```

**Validation error response (400):**
```json
{
  "success": false,
  "message": "Validation failed",
  "errors": [
    { "field": "email", "message": "Please provide a valid email address" }
  ]
}
```

---

## ☁️ Deployment Guide

You'll deploy the **backend** and **frontend** separately since they're different types of apps (a Node server vs. static files). A common combo: **backend → Render or Vercel**, **frontend → Netlify or Vercel**.

### Option A — Deploy Backend to Render (recommended for Express + MongoDB apps)

Render is generally more reliable than Vercel for long-running Express servers.

1. Push your `backend/` folder to a GitHub repository.
2. Go to [render.com](https://render.com) → New → **Web Service**.
3. Connect your GitHub repo, select the `backend` folder as the root directory.
4. Set:
   - **Build Command:** `npm install`
   - **Start Command:** `npm start`
5. Add environment variables under "Environment":
   - `MONGO_URI` = your MongoDB Atlas connection string
   - `NODE_ENV` = `production`
   - `CLIENT_ORIGIN` = your deployed frontend URL (e.g. `https://your-portfolio.netlify.app`)
6. Click **Create Web Service**. Once deployed, you'll get a URL like:
   `https://portfolio-backend.onrender.com`

### Option B — Deploy Backend to Vercel

1. Push the `backend/` folder to GitHub.
2. Go to [vercel.com](https://vercel.com) → **New Project** → import your repo.
3. Set the **Root Directory** to `backend`.
4. Vercel will detect `vercel.json` (already included) and deploy it as a serverless function.
5. Add Environment Variables in the Vercel dashboard:
   - `MONGO_URI`
   - `NODE_ENV=production`
   - `CLIENT_ORIGIN` = your frontend URL
6. Deploy. You'll get a URL like `https://portfolio-backend.vercel.app`.

> ⚠️ Note: Vercel's serverless functions are stateless and can have cold starts, so for a persistently-running Express + MongoDB app, **Render** or **Railway** tends to be smoother. Both options are provided so you can pick what your internship program expects.

### Step 2 — Set up MongoDB Atlas (required for any live deployment)

Local MongoDB only works on your own machine, so for deployment you need a cloud database:

1. Create a free cluster at [MongoDB Atlas](https://www.mongodb.com/cloud/atlas/register).
2. Under **Database Access**, create a database user with a username/password.
3. Under **Network Access**, add `0.0.0.0/0` (allow access from anywhere) — fine for a demo/internship project.
4. Under **Database → Connect → Drivers**, copy your connection string, e.g.:
   `mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/portfolioDB`
5. Use this as `MONGO_URI` in your backend's environment variables (both locally and in production).

### Option C — Deploy Frontend to Netlify

1. Push the `frontend/` folder to GitHub (or drag-and-drop it directly).
2. Go to [netlify.com](https://netlify.com) → **Add new site** → "Import an existing project" (or "Deploy manually" to drag-and-drop the folder).
3. If importing from GitHub, set:
   - **Base directory:** `frontend`
   - **Build command:** (leave blank)
   - **Publish directory:** `frontend` (or `.` if base directory is already `frontend`)
4. Before deploying, update `frontend/js/script.js`:
   ```js
   const API_BASE_URL = "https://your-backend-url.onrender.com/api";
   ```
   Replace with your actual deployed backend URL from Step 1.
5. Click **Deploy site**. You'll get a live URL like `https://your-portfolio.netlify.app`.

### Option D — Deploy Frontend to Vercel

1. Push `frontend/` to GitHub.
2. Go to [vercel.com](https://vercel.com) → **New Project** → import the repo.
3. Set **Root Directory** to `frontend`.
4. Framework preset: choose **Other** (it's a static site, no build step).
5. Update `API_BASE_URL` in `script.js` (same as above) before deploying.
6. Deploy — you'll get a URL like `https://your-portfolio.vercel.app`.

### Final Step — Update CORS on the Backend

Once your frontend has a live URL, go back to your backend's environment variables (on Render/Vercel) and update:

```
CLIENT_ORIGIN=https://your-portfolio.netlify.app
```

Then redeploy/restart the backend so it accepts requests from your live frontend.

---

## ✅ Quick Checklist Before Submitting as an Internship Assignment

- [ ] Update personal info in `frontend/index.html` (name, email, phone, social links, bio)
- [ ] Replace placeholder projects in `frontend/js/script.js` with your real projects and links
- [ ] Update education/certification details in the `timelineData` array in `script.js`
- [ ] Add your resume PDF to `frontend/assets/resume.pdf` (linked from the Resume button)
- [ ] Set up MongoDB Atlas and confirm the contact form saves data in production
- [ ] Deploy backend (Render/Vercel) and frontend (Netlify/Vercel)
- [ ] Update `API_BASE_URL` in `script.js` to point to your deployed backend
- [ ] Update `CLIENT_ORIGIN` in the backend's environment variables to your deployed frontend URL
- [ ] Test the live site end-to-end: submit the contact form and confirm it appears in MongoDB
- [ ] Push the whole project to a public GitHub repository and include the link in your submission

---

## 🧪 Troubleshooting

| Issue | Likely Cause | Fix |
|---|---|---|
| "MongoDB connection error" | Wrong `MONGO_URI` or MongoDB not running | Check your `.env`, confirm local MongoDB service is running, or verify Atlas credentials/IP whitelist |
| Contact form shows "Failed to send message" | Backend not running, or wrong `API_BASE_URL` | Ensure backend is running on the expected port; check the URL in `script.js` |
| CORS error in browser console | Frontend origin not allowed by backend | Add your frontend's URL to `CLIENT_ORIGIN` in the backend `.env` |
| Form submits but nothing appears in MongoDB | Wrong database/collection name, or Atlas IP not whitelisted | Double-check `MONGO_URI` includes the correct database name; whitelist your IP (or `0.0.0.0/0`) in Atlas |

---

## 📄 License

This project is free to use for personal, educational, and internship submission purposes.
