---
layout: default
title: Setting up local development
parent: Tutorials
nav_order: 4
permalink: /tutorials/setup
---

# Setting up local development

To get started with local development, you will first need to add files to the frontend and backend directories for environment variables.

* Notes that these files are and should not be committed to the repository because they contain sensitive API keys and other secret information.  You can find these secrets in our shared Google drive.

In the `/frontend` directory, add a file named `.env`. 

- To use your local backend for frontend API calls, ensure that `REACT_APP_BACKEND_URL` is set to `'http://localhost:5000'`

In the `/backend` directory, add a file name `config.env`.

- For the backdoor login to function properly, set `BASE_URL` to `http://localhost:3000`
- Ensure that `ATLAS_URI` has the word `dev` after the slash and before the question mark
- Note `BACKDOOR_PASSWORD` as you will need this for backdoor authentication

Now that your environment variables are setup, we can begin to develop locally.

1. Open a terminal (you can use the integrated terminal in your IDE) and start the backend server

```
cd backend && node index.js
```

2. Open another terminal window (keeping the backend server running) and start the frontend website

```
cd frontend && npm start
```

If you don't want the website to automatically open, you can use this command instead:

```
cd frontend && export BROWSER=none && npm start
```

3. Navigate to `http://localhost:3000/login/backdoor`.  Enter your JHED and the backdoor password found in `backend/config.env`

You are now authenticated for local development.  
Code changes made to frontend code will be updated automatically, but changing an environment variable will require you to restart `npm start`.  
If you are testing admin endpoints or trying to access the admin page, you will need to change your isAdmin field in the `dev` database via MongoDB.
