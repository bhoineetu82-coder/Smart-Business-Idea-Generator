# Smart Business Idea Generator

A beginner-friendly AI agent that generates personalised business ideas using **IBM Granite** via **IBM Cloud watsonx.ai**.

Built as a learning project by CSE students. The goal is to understand how to integrate a real Large Language Model (LLM) into a simple full-stack web application.

---

## Features

- **7-field input form** — Business Domain, Budget, Interests, Skills, Target Audience, Goals, Preferences
- **Real AI generation** — Calls IBM Granite via IBM Cloud. No hardcoded or fake results.
- **4 structured ideas per query** — Each idea includes Title, Description, Problem, Solution, Target Users, Estimated Investment, Revenue Model, USP, Key Challenge, and Feasibility Score
- **Best-Fit Recommendation** — Automatically highlights the highest-scoring idea
- **Responsive dashboard** — Works on desktop, tablet, and mobile
- **Proper error handling** — Validates form inputs and shows clear messages for API errors
- **No database required** — Uses `localStorage` to pass data between pages

---

## Technology Stack

| Layer | Technology | Purpose |
|---|---|---|
| **Frontend** | HTML5 + CSS3 + Vanilla JavaScript | UI pages and form handling |
| **Backend** | Node.js + Express.js | Web server and API route |
| **AI Model** | IBM Granite (`ibm/granite-13b-instruct-v2`) | Generates business ideas |
| **AI Platform** | IBM Cloud watsonx.ai | Hosts and runs the Granite model |
| **HTTP Client** | axios | Backend calls to IBM Cloud REST API |
| **Secrets** | dotenv | Loads API credentials from `.env` file |
| **CORS** | cors | Allows browser to call the backend |

---

## Project Structure

```
smart-business-idea-generator/
│
├── backend/
│   ├── server.js                ← Entry point. Starts Express server on port 3000.
│   ├── .env                     ← YOUR SECRETS (never commit — excluded by .gitignore)
│   ├── .env.example             ← Template showing which variables are needed
│   │
│   ├── routes/
│   │   └── generate.js          ← POST /api/generate — validates input, calls Granite
│   │
│   ├── services/
│   │   └── granite.js           ← IBM Cloud authentication + Granite API call + response parsing
│   │
│   └── utils/
│       └── promptBuilder.js     ← Builds the structured prompt from user form data
│
├── frontend/
│   ├── index.html               ← Page 1: Input form with 7 fields
│   ├── results.html             ← Page 2: Dashboard showing generated idea cards
│   │
│   ├── css/
│   │   └── styles.css           ← All responsive styles for both pages
│   │
│   └── js/
│       ├── form.js              ← Form validation, fetch to backend, localStorage save
│       └── results.js           ← Reads localStorage, renders idea cards, best-fit logic
│
├── .gitignore                   ← Excludes node_modules/ and backend/.env
├── package.json                 ← Project metadata and dependencies
└── README.md                    ← This file
```

---

## Prerequisites

Before running this project, install the following:

### 1. Node.js (required)

Download and install the **LTS version** from [https://nodejs.org](https://nodejs.org).

After installation, verify in your terminal:
```
node --version    # e.g. v20.x.x
npm --version     # e.g. 10.x.x
```

### 2. IBM Cloud Account + watsonx.ai (required for AI generation)

Follow the step-by-step guide in the section below.

---

## IBM Cloud + watsonx.ai Setup

> **This is required** before the AI generation will work. Without valid credentials, the app will show a configuration error when you click "Generate".

### Step 1 — Create a Free IBM Cloud Account

1. Go to [https://cloud.ibm.com/registration](https://cloud.ibm.com/registration)
2. Sign up with your email (free tier available — no credit card needed initially)
3. Verify your email and log in

### Step 2 — Create a watsonx.ai Project

1. Go to [https://dataplatform.cloud.ibm.com](https://dataplatform.cloud.ibm.com)
2. Click **"New project"** → **"Create an empty project"**
3. Give it any name (e.g. `business-idea-generator`)
4. Click **Create**
5. After creation, go to **Manage** tab → **General**
6. Copy the **Project ID** shown — you will need it in Step 5

### Step 3 — Add Watson Machine Learning to Your Project

1. In IBM Cloud dashboard, click **"Create resource"**
2. Search for **"Watson Machine Learning"**
3. Select the **Lite (free)** plan
4. Choose region **us-south**
5. Click **Create**
6. Return to your watsonx.ai project → **Manage** tab → **Services and integrations**
7. Click **Associate service** and select the Watson Machine Learning instance you just created

### Step 4 — Generate an IBM Cloud API Key

1. Go to [https://cloud.ibm.com/iam/apikeys](https://cloud.ibm.com/iam/apikeys)
2. Click **"Create an IBM Cloud API key"**
3. Give it a name (e.g. `granite-key`) and click **Create**
4. **COPY THE KEY IMMEDIATELY** — IBM does not show it again after you close this dialog
5. Store it somewhere safe temporarily

### Step 5 — Note Your Details

You will need these three values in the next section:

| Variable | Where to get it |
|---|---|
| `IBM_API_KEY` | The API key you copied in Step 4 |
| `IBM_PROJECT_ID` | The Project ID from Step 2 |
| `IBM_REGION` | `us-south` (or your account region) |

---

## Installation

### 1. Clone or download the project

```
cd "C:\Users\pc\OneDrive\Desktop\ibm session\smart-business-idea-generator"
```

### 2. Install dependencies

```
npm install
```

This creates the `node_modules/` folder with all required packages:
- `express` — web server
- `axios` — HTTP client for IBM Cloud calls
- `dotenv` — loads secrets from `.env`
- `cors` — cross-origin resource sharing

### 3. Create your `.env` file

```
copy backend\.env.example backend\.env
```

Then open `backend\.env` in any text editor and replace the placeholder values:

```env
IBM_API_KEY=your_actual_api_key_here
IBM_PROJECT_ID=your_actual_project_id_here
IBM_REGION=us-south
PORT=3000
```

> ⚠ **Never commit `backend/.env` to git.** It is already excluded by `.gitignore`.

---

## Running the Application

```
npm start
```

You should see:
```
==============================================
 Smart Business Idea Generator — Running!
  Local:   http://localhost:3000
==============================================
```

Open your browser and go to: **[http://localhost:3000](http://localhost:3000)**

---

## How to Use the Application

1. **Fill in the input form** (all 7 fields are required):
   - Business Domain — select from dropdown
   - Available Budget — select from dropdown
   - Your Interests — type comma-separated interests
   - Your Skills — type comma-separated skills
   - Target Audience — who your business will serve
   - Business Goals — what you want to achieve
   - Additional Preferences — any other constraints

2. **Click "✨ Generate Business Ideas"** — the button shows a spinner while IBM Granite processes your request (usually 10–30 seconds)

3. **View your ideas** on the results dashboard:
   - Each idea is shown as a card with 9 fields
   - The best-fit idea (highest feasibility score) is highlighted in green with a 🏆 badge
   - Use "← Back to Form" to change your inputs
   - Use "↺ Generate New Ideas" to start fresh

---

## Testing the Application

### Test 1 — Form Validation (no credentials needed)

1. Open `http://localhost:3000`
2. Click "Generate Business Ideas" without filling anything in
3. **Expected:** Red error message under each empty field

### Test 2 — Live Error Clearing (no credentials needed)

1. Trigger the validation errors (Test 1)
2. Start typing in any red-highlighted field
3. **Expected:** The red border and error message disappear immediately

### Test 3 — API Error Display (no credentials needed)

1. Fill in all 7 fields and click "Generate"
2. If `backend/.env` is not configured yet:
3. **Expected:** A red banner appears: "IBM_API_KEY is not configured. Please copy backend/.env.example..."

### Test 4 — Results Dashboard Error State (no credentials needed)

1. Open `http://localhost:3000/results.html` directly (without going through the form)
2. **Expected:** Error message: "It looks like you haven't generated any ideas yet. Please fill in the form first."

### Test 5 — Dashboard with Test Data (no credentials needed)

1. Open `http://localhost:3000/results.html`
2. Open browser DevTools → **Console** tab
3. Paste and run this JavaScript:

```javascript
localStorage.setItem('generatedIdeas', JSON.stringify([
  {
    title: "EduSnap",
    description: "A micro-learning platform for students.",
    targetAudience: "College students",
    problem: "Long study sessions with poor retention.",
    solution: "5-minute video lessons with quizzes.",
    estimatedInvestment: "Under $500",
    revenueModel: "Freemium — premium subscription at $5/month",
    usp: "Bite-sized, gamified learning that fits in a commute.",
    challenges: "High-quality content creation at scale.",
    feasibilityScore: 9
  },
  {
    title: "FitCoach AI",
    description: "Personalised home workout planner.",
    targetAudience: "Working adults aged 25–40",
    problem: "No time or money for a personal trainer.",
    solution: "AI-generated 10-min workout plans.",
    estimatedInvestment: "$500 – $2,000",
    revenueModel: "Monthly subscription at $8/month",
    usp: "No equipment needed. Adapts to your schedule daily.",
    challenges: "User retention after the first month.",
    feasibilityScore: 7
  }
]));
localStorage.setItem('userInput', JSON.stringify({
  domain: "Technology", budget: "Under $500", businessGoals: "Passive income"
}));
location.reload();
```

4. **Expected:** 2 idea cards render. EduSnap is highlighted as 🏆 Best Fit. Summary pills show Domain, Budget, Goals.

### Test 6 — Full End-to-End (requires IBM credentials)

1. Configure `backend/.env` with your real IBM Cloud credentials
2. Run `npm start`
3. Open `http://localhost:3000`
4. Fill in all fields and click "Generate Business Ideas"
5. Wait 10–30 seconds for Granite to respond
6. **Expected:** Redirected to `results.html` showing 4 AI-generated idea cards

---

## How the IBM Granite Integration Works

```
User Form Submit
      ↓
form.js → POST /api/generate (JSON body)
      ↓
routes/generate.js → validates 7 fields
      ↓
utils/promptBuilder.js → builds a structured prompt string
      ↓
services/granite.js → Step A: exchanges IBM_API_KEY for a temporary IAM token
                              POST https://iam.cloud.ibm.com/identity/token
      ↓
services/granite.js → Step B: calls Granite with the prompt
                              POST https://us-south.ml.cloud.ibm.com/ml/v1/text/generation
      ↓
services/granite.js → parseIdeas() extracts JSON array from response text
      ↓
routes/generate.js → returns { ideas: [...] } to the browser
      ↓
form.js → saves to localStorage → redirects to results.html
      ↓
results.js → reads localStorage → renders idea cards
```

The IAM Bearer Token expires after approximately 1 hour. The application fetches a fresh token on every request, so this is handled automatically.

---

## Common Errors and Solutions

| Error Message | Cause | Solution |
|---|---|---|
| `IBM_API_KEY is not configured` | `.env` file missing or not set up | Run `copy backend\.env.example backend\.env` and fill in values |
| `IBM IAM authentication failed (HTTP 400)` | API key is wrong or expired | Generate a new API key at [cloud.ibm.com/iam/apikeys](https://cloud.ibm.com/iam/apikeys) |
| `IBM Granite API call failed (HTTP 404)` | Wrong Project ID | Check your project ID at [dataplatform.cloud.ibm.com](https://dataplatform.cloud.ibm.com) |
| `IBM Granite API call failed (HTTP 403)` | WML service not associated | Associate Watson Machine Learning service with your project |
| `Granite did not return a valid JSON array` | Model output was truncated or malformed | Try again — Granite occasionally produces incomplete output |
| `npm: command not found` | Node.js is not installed | Install from [nodejs.org](https://nodejs.org) and restart terminal |

---

## Project Limitations (Intentional — Beginner Scope)

- **No database** — Ideas are stored in `localStorage` only. Closing the browser clears them.
- **No user accounts** — No login, no saved history
- **No streaming** — The page waits for the full Granite response before displaying anything
- **English only** — The prompt and UI are in English only
- **4 ideas per query** — Hardcoded in the prompt for consistency

These are deliberate design choices to keep the codebase small and learnable.

---

## Security Notes

- `backend/.env` is excluded from git via `.gitignore` — your API key will never be committed
- `backend/.env.example` contains only placeholder values — safe to commit
- The API key is never sent to the browser — all IBM Cloud calls happen on the backend only
- User input is validated on both frontend (form.js) and backend (routes/generate.js)

---

## Authors

CSE Students — Built as a learning project using IBM Bob and IBM Granite.
