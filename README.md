# Canvas Student Assignment Tracker

This project is a full-stack web application that integrates with the Canvas LMS API to help students track assignments, tests, syllabus documents, file uploads, and prioritize their workload with a clean user interface.

## 🌐 Tech Stack

### Frontend:
- [Next.js](https://nextjs.org/) (React framework)
- [Tailwind CSS](https://tailwindcss.com/) (utility-first CSS)
- [NextAuth.js](https://next-auth.js.org/) (authentication with Canvas OAuth2)

### Backend:
- [Node.js](https://nodejs.org/) + [Express](https://expressjs.com/)
- [Prisma ORM](https://www.prisma.io/)
- [PostgreSQL](https://www.postgresql.org/)
- [Axios](https://axios-http.com/) for Canvas API calls

### Deployment:
- Frontend: [Vercel](https://vercel.com/)
- Backend/DB: [Render](https://render.com/) or [Railway](https://railway.app/)

---

## 🔐 Canvas OAuth Setup

1. Go to your Canvas LMS (e.g., `https://school.instructure.com`) and register a **Developer Key**.
   - Redirect URI: `http://localhost:3000/api/auth/callback/canvas`
   - Note your **Client ID** and **Client Secret**.

2. Create a `.env.local` file in your root:

```env
CANVAS_CLIENT_ID=your_client_id
CANVAS_CLIENT_SECRET=your_client_secret
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=your_random_string
```

3. Set up NextAuth in `pages/api/auth/[...nextauth].ts`:

```ts
import NextAuth from "next-auth";
import CanvasProvider from "next-auth/providers/oauth";

export default NextAuth({
  providers: [
    CanvasProvider({
      id: "canvas",
      name: "Canvas",
      type: "oauth",
      clientId: process.env.CANVAS_CLIENT_ID,
      clientSecret: process.env.CANVAS_CLIENT_SECRET,
      authorization: "https://canvas.instructure.com/login/oauth2/auth",
      token: "https://canvas.instructure.com/login/oauth2/token",
      userinfo: "https://canvas.instructure.com/api/v1/users/self/profile",
      profile(profile) {
        return {
          id: profile.id,
          name: profile.name,
          email: profile.primary_email,
          image: profile.avatar_url,
        };
      },
    }),
  ],
  secret: process.env.NEXTAUTH_SECRET,
});
```

---

## 📦 Backend Setup (Express API)

1. Install dependencies:
```bash
npm install express axios dotenv cors
```

2. Set up a basic Express server in `api/index.js`:
```js
const express = require('express');
const cors = require('cors');
const dotenv = require('dotenv');
dotenv.config();

const app = express();
app.use(cors());
app.use(express.json());

app.get('/assignments', async (req, res) => {
  // Add Canvas API logic with req.headers.authorization
});

const PORT = process.env.PORT || 4000;
app.listen(PORT, () => console.log(`API running on ${PORT}`));
```

---

## 🗂️ Features

- ✅ Canvas OAuth2 authentication (NextAuth)
- 📆 Assignment tracking from multiple courses
- 🧪 Quiz/test integration from Canvas quizzes API
- 📄 Syllabus and file listing with Canvas file endpoints
- 🎯 Custom priority tagging per user
- 📊 Dashboard view by due date, urgency, or course
- 🔔 Notification scheduler (coming soon)

---

## 🧪 Local Dev Setup

```bash
git clone https://github.com/yourusername/canvas-tracker.git
cd canvas-tracker
npm install
npm run dev
```

Backend:
```bash
cd api
npm install
node index.js
```

---

## 📚 Resources
- [Canvas API Docs](https://canvas.instructure.com/doc/api/)
- [NextAuth Canvas OAuth Guide](https://next-auth.js.org/providers/oauth)
- [Prisma Quickstart](https://www.prisma.io/docs/getting-started)

---

## 🧠 Contributions
PRs, issues, and feature ideas are welcome! Focus areas:
- Offline assignment tracking
- Mobile responsive improvements
- Google Calendar integration

---

## 📄 License
MIT License

---

Made with ❤️ for students who want to stay ahead of deadlines.

---

## 🚀 Demo (coming soon)
Live URL: in progress

