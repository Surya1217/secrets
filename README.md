Secrets — Google-Login-Enabled App

Secrets is a web application built with Node.js and Express that allows users to log in using their Google account (via OAuth 2.0). Once authenticated, users can access protected content — no username/password pairs to manage, just “Sign in with Google”.

The primary goal is to provide a secure, simple, and user-friendly login flow using third-party authentication, avoiding the need to manage passwords or user credentials directly.

Tech Stack

Backend: Node.js + Express

Authentication: OAuth 2.0 via Google + passport / passport-google-oauth20 (or similar)

View Engine / Frontend: EJS templates / HTML

Session Management: express-session

Security: Sensitive keys and secrets managed via environment variables (.env), not committed to Git

Why Google OAuth Login

Eliminates need for users to create and remember new credentials — they login with their existing Google account.

Google handles account verification and security — your app never manages raw passwords.

Reduces barrier to entry: sign-in is fast and familiar for users.

More secure than basic custom auth (if implemented properly) — trust Google’s identity infrastructure.

Conceptually, this uses the OAuth 2.0 authorization flow: user is redirected to Google for consent → upon approval Google redirects back to your app with a token → your app verifies the token and establishes a session. 
Google for Developers
+2
eddy.hashnode.dev
+2

Getting Started — Setup & Run Locally
Prerequisites

Node.js and npm installed

A Google account

A Google OAuth 2.0 Client ID and Client Secret (via Google Cloud Console) — don’t commit these to the repo

Steps
# 1. Clone repo
git clone https://github.com/Surya1217/secrets.git
cd secrets

# 2. Install dependencies
npm install

# 3. Create .env file in project root and add credentials:
#    (example:)
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
SESSION_SECRET=someRandomSecretForExpressSession

# 4. Start the server (development)
npm run dev
# or
node index.js

# 5. Open in browser
http://localhost:3000

Google OAuth Setup (if not done)

Go to [Google Cloud Console → APIs & Services → Credentials]. 
LoginRadius
+1

Create a new OAuth 2.0 Client ID (Application type = Web application).

For Authorized Redirect URIs, add something like:

http://localhost:3000/auth/google/callback


Copy the Client ID & Secret. Put them into your .env as described.

Authentication Flow Summary
Step	Description
User clicks “Login with Google”	Browser redirected to Google’s OAuth consent screen
User approves consent	Google redirects back to your app with an authorization code or token
App exchanges code/token	Verifies with Google and retrieves user info (email, profile)
App creates session	Uses express-session + Passport (or similar) to track login state
User accesses protected pages	Only authenticated sessions can see secure content

This flow is implemented using passport-google-oauth20 (or equivalent), simplifying OAuth integration. 
GeeksforGeeks
+1

What’s Included

Sign-in with Google (OAuth 2.0)

Log-in with Google (OAuth 2.0)

Session-based authentication (so users stay logged in between page loads)

Protected routes / content — accessible only to logged-in users

Logout functionality (to end session)

Sample UI (login page + protected page)

Security & Best Practices

Do not commit sensitive credentials (Client ID, Client Secret, session secrets) — use .env.

Use a .gitignore to ensure secret files (e.g. .env) are not tracked.

Use HTTPS / secure cookies when deploying to production.

Validate callback URLs in Google Console — only authorize URIs you control.

Handle session store appropriately (memory store may not be suited for production)

Possible Extensions / Improvements

Persist user info in a database (PostgreSQL, MongoDB) for managing profiles or user-specific data

Add other OAuth providers (GitHub, Facebook, etc.) using Passport’s strategy model

Add logout, session expiry, session refresh handling

Enforce privacy / consent screens appropriately if deploying publicly

Add UI enhancements — better login page, user profile page, etc.

Example Project Structure
/ (project root)
│   index.js                # Main server file
│   package.json
│   .env                    # (not committed) — holds CLIENT_ID, CLIENT_SECRET, etc.
│   .gitignore
│
├── /views                  # EJS templates
│     ├── login.ejs         # Login page with “Sign in with Google” button
│     ├── protected.ejs     # Example protected content page
│     └── error.ejs         # (optional) error / access denied pages
│
└── /public                 # Static files (CSS, images, optional)

How to Use (from end-user perspective)

Open the app in browser → / or /login

Click “Sign in with Google”

Grant permission via Google consent screen

Upon success — you’ll see protected content / be logged in

You can log out if needed (via “Logout” link/button)
