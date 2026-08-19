# NotesApp — Full-Stack React on AWS

This repository is the companion codebase for an **AWS hands-on tutorial** that
walks you through building, deploying, and iterating on a full-stack React
application. You will create a React app, host the source on GitHub, deploy it
to the cloud with **AWS Amplify**, make code changes, redeploy, and push updates
to GitHub — all the steps needed to go from a local development project to a
continuously deployed web application.

---

## What you will learn

By the end of this tutorial you will have:

- **Created a React app** — a modern, production-ready single-page application
  built with Vite + React.
- **Initialized a GitHub repository** — local version control wired up to a
  public GitHub repo over SSH.
- **Deployed your app with AWS Amplify** — connected GitHub to AWS Amplify so
  every push triggers an automatic build and deploy.
- **Implemented code changes and deployed your app** — edited the app locally,
  pushed the change, and let Amplify redeploy automatically.
- **Pushed to GitHub** — used Git to commit and push changes that drive the
  continuous deployment pipeline.

---

## Prerequisites

Before you begin, make sure you have the following:

| Tool | Why you need it |
| --- | --- |
| [Node.js](https://nodejs.org/) (v18+) | Run the React dev server and build |
| [Git](https://git-scm.com/) | Version control and pushing to GitHub |
| [GitHub CLI](https://cli.github.com/) (`gh`) | Create the GitHub repo and connect Amplify |
| An [AWS account](https://aws.amazon.com/) | Host the app with AWS Amplify |
| [Set up SSH for GitHub](https://docs.github.com/en/get-started/getting-started-with-git/using-ssh) | Authenticate Git operations over SSH |

---

## Tutorial overview

### 1. Create a React app

Start by scaffolding the React application with Vite:

```bash
# Create the project
npm create vite@latest notesapp -- --template react

# Move into the project directory
cd notesapp

# Install dependencies
npm install

# Run the development server
npm run dev
```

Open `http://localhost:5173` to see the app running locally. The scaffold creates
a working React app with Hot Module Replacement (HMR) and a set of ESLint rules
out of the box.

> This repository was generated from the **Vite + React** template, which is the
> starting point for the rest of the tutorial.

### 2. Initialize a GitHub repo

Put the project under version control and publish the source to GitHub:

```bash
# Initialize a local Git repository (default branch: main)
git init

# Add all files (node_modules and build output are ignored via .gitignore)
git add .

# Commit the initial app
git commit -m "Initial commit: create React app"

# Create a public GitHub repository named "notesapp" and push over SSH
# (requires gh to be logged in: gh auth login)
gh repo create notesapp --public --source=. --push
```

If everything succeeds, the `--push` flag uploads your code and the repository is
available at `https://github.com/<YOUR_USERNAME>/notesapp`. The repository is
**public**, so anyone can view the source.

### 3. Deploy your app with AWS Amplify

With the code on GitHub, deploy it to AWS Amplify:

1. Sign in to the [AWS Management Console](https://console.aws.amazon.com/).
2. Open the **AWS Amplify** console.
3. Choose **Get started** under *Build*.
4. Select **GitHub** as the repository provider and authorize AWS to access your
   repositories.
5. Pick the `notesapp` repository and the branch you pushed (e.g. `main`).
6. Accept the default build settings, or point Amplify at the build
   specification below:

   ```json
   {
     "version": 1,
     "frontend": {
       "phases": {
         "build": {
           "commands": ["npm run build"]
         }
       },
       "artifacts": {
         "baseDirectory": "dist",
         "files": ["**/*"]
       },
       "cache": {
         "paths": ["node_modules/**/*"]
       }
     }
   }
   ```

7. Choose **Save and deploy**.

Amplify provisions a CI/CD pipeline, builds the React app, and serves it on a
globally distributed content URL — no servers to manage. Deployment typically
takes a few minutes.

### 4. Implement code changes and deploy our app

Now make a change and watch the app redeploy automatically:

```bash
# Edit the app locally, for example updating the header text in src/App.jsx
# ...make your change...

# Commit and push the change
git add .
git commit -m "Update app header text"
git push
```

Because Amplify is connected to your GitHub branch, pushing a change triggers a
new build and deploy automatically. Refresh the live Amplify URL to see your
updated app.

### 5. Push to GitHub

Throughout the tutorial, use Git to keep your local work and the remote
repository in sync:

```bash
# See what changed
git status

# Stage the changes you want to include
git add .

# Record the change with a descriptive message
git commit -m "Describe your change here"

# Push to GitHub and trigger the Amplify redeploy
git push
```

Every `git push` to the connected branch updates the deployed application.

---

## Project structure

```
notesapp/
├── public/              # Static assets (favicon, icons)
├── src/                 # React source code
│   ├── App.jsx          # Root application component
│   ├── App.css          # Component styles
│   ├── index.css        # Global styles
│   ├── main.jsx         # React entry point
│   └── assets/          # Images and media
├── index.html           # HTML shell
├── vite.config.js       # Vite configuration
├── eslint.config.js     # ESLint configuration
├── package.json         # Project manifest and scripts
└── README.md            # This file
```

---

## Built-in scripts

| Command | What it does |
| --- | --- |
| `npm run dev` | Start a local development server with HMR |
| `npm run build` | Bundle the app for production in `dist/` |
| `npm run preview` | Preview the production build locally |
| `npm run lint` | Run ESLint to check the codebase |

---

## Next steps

Once your app is live on Amplify, try extending it:

- Add a backend API and data store with **AWS Amplify** (`@aws-amplify/*`).
- Add **authentication** with Amazon Cognito.
- Attach a custom domain with **Amazon Route 53**.
- Monitor performance and errors with **Amazon CloudWatch**.

---

## License

This tutorial content is provided as-is for learning purposes.
