# Repository Description

This artifact is the replication package for our paper entitled: "GitRev: A Gamification-based Approach for Engagement in Modern Code Review Activities"

This replication package contains the following folders:

myserver: The Node.js server code for authentication and data management. https://github.com/jaykay9999/GitRev/tree/main/myserver

fastapi: The Python FastAPI scripts interfacing with ChatGPT for activity rating. https://github.com/jaykay9999/GitRev/tree/main/fastapi

react-extension: The React.js-based Chrome extension code for the User Interface. https://github.com/jaykay9999/GitRev/tree/main/react-extension

probot: The Probot app code for GitHub activity tracking. https://github.com/jaykay9999/GitRev/tree/main/probot

Refer to the included setup guide and demo video for detailed instructions, and check the GitRev_prompts.md file for the evaluation scripts used within the rewarding system.


# GitRev Gamification System

GitRev is a comprehensive gamification system designed to enhance the GitHub UI dashboard and user experience. It integrates a GitHub app, GitHub authentication, a Chrome extension, a MongoDB database, a Gamification server, and a FastAPI application to provide an engaging and interactive environment for GitHub users.


![alt text](system_design.png)

## Getting Started

These instructions will guide you through setting up GitRev both locally and on the cloud.

### Prerequisites

Before you begin, ensure you have the following accounts and tools installed:
- A GitHub account
- MongoDB account for database creation
- Node.js installed for running server-side code
- Python installed for running FastAPI application

### Local Setup

#### 1. GitHub App (Folder: `probot`)


This guide will walk you through creating a GitHub App and configuring it to work with Probot.

## Step 1: Create Your GitHub App

1. **Developer Settings**:
   - Go to your GitHub account **Settings**.
   - Click on **Developer settings** > **GitHub Apps** > **New GitHub App**.

2. **App Configuration**:
   - Enter your app's name and Homepage URL.
   - Configure your Webhook URL (optional for development).
   - Generate and download your private key securely.

3. **Permissions & Events**:
   - Set necessary permissions and event subscriptions.
   - Click **Create GitHub App**.

## Step 2: Configure Probot

1. **Clone and Prepare**:
   - Clone your Probot app repository to your machine.

2. **Environment Variables**:
   - Navigate to the `probot` folder.
   - Create or edit the `.env` file with the following variables:

   ```env
    APP_ID, PRIVATE_KEY, WEBHOOK_SECRET, GITHUB_CLIENT_ID, and GITHUB_CLIENT_SECRET are provided by GitHub upon app creation.
    PORT is the local port where your Probot app will run.
    ATLAS_URI is your MongoDB Atlas connection string, obtainable from MongoDB Atlas.
    SERVER_API_BASE_URL is your gamification server URL.
    FAST_API_BASE_URL is the base URL for your FastAPI app, typically obtained after running it with uvicorn

Replace placeholder values with those specific to your app.

 Install & Build:
 Run npm install and npm run build in the probot directory.
 Step 3: Run Your App
 Execute npm start to run your Probot app.
 For more details, you can read the Probot documentation : https://probot.github.io/docs/configuration/

 Note: The .env file already contains the necessary variables. You need to fill in the actual values provided by GitHub.



#### 3. Chrome Extension

To set up the Chrome extension that gamifies GitHub, follow these steps:

### Prepare the Environment
- Ensure you have [Node.js](https://nodejs.org/) and [Yarn](https://yarnpkg.com/) installed on your system.

### Build the Extension
1. Navigate to the directory containing the Chrome extension.
2. Run `yarn install` to install all dependencies, including the necessary node modules.
3. Execute `yarn run build` to compile the extension and generate the `dist` folder. This folder contains the build artifacts that Chrome will load.

### Load the Extension into Google Chrome
1. Open Google Chrome and navigate to `chrome://extensions/`.
2. Enable "Developer mode" by toggling the switch in the top-right corner.
3. Click on "Load unpacked" and select the `dist` folder generated by the build process.

### Run the Extension in Watch Mode
- To actively develop and test the extension with live reloading, run `yarn run watch` from the extension's directory. This command will automatically rebuild the extension when source files change.
- Ensure your server is running on `localhost:3002` for the extension to connect and function properly.

### Client ID and Server IP Address Configuration
- **Client ID:** Locate the `client_id` in `background.ts`, which can be found in the GitHub developer settings.
- **Server IP Address:** Configure the `.env` file within the extension's directory to set the server IP address, defaulting to `localhost:3002`.

### Authentication and Login
- Upon using the extension for the first time, a login will be required. This login process is facilitated through the server API call and GitHub authentication app, ensuring secure access and integration with GitHub.


#### 4. MongoDB Database Creation
- Sign up or log in to [MongoDB Cloud](https://www.mongodb.com/products/platform/cloud) to create a free 500 MB cluster.
- Navigate to your created cluster, then to "Database" in the left menu.
- Click on "Connect", select the "Drivers" option, and copy the URI code. This URI enables the connection between your Node.js gamification server and the MongoDB database.
- **Example URI Format:**
mongodb+srv://<username>:<password>@cluster0.j4sk7x6.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0

#### 5. Gamification Server (Folder: `myserver`)
- Navigate to `/myserver/.env` and configure the following variables:
- `CLIENT_ID`: Obtain this from your GitHub app settings under Developer Settings.
- `CLIENT_SECRET`: Also found in your GitHub app settings.
- `ATLAS_URI`: Use the MongoDB URI from your cluster setup.

Naviage back to `/myserver`
- Install dependencies with the command:
npm install
- Run the server with the command:
npm start
This command initiates `server.js` on `localhost:3002`.

#### 6. FastAPI App (Folder: `FastAPI`)
Navigate to `/FastAPI/main.py` and replace the chatgpt api key string with your obtained api key from OpenAi , also replace the MongoDB URI with the URI from your cluster dashboard, and replace the server_api with your gamification server address (by default localhost:3002)

- Navigate back to `/FastAPI` and run the command:
pip install -r requirements.txt

- Then run the command: 
uvicorn main:app --reload

This command starts the FastAPI application, making it accessible on port `8000`.



#### 7. GitRev Usage

Once the full system is running, the probot app will start tracking your GitHub activities and update your earned points in the database through "myserver" and "fastapi"

To access the gamification elements (badges, leaderboards, etc...) you should go to https://github.com and make sure that you are logged in. You will find a new purple button on top right that allows you to automatically create an account in 1 click. after account creation, you should be able to view all the game elements directly on your GitHub dashboard. This video shows hows the system works after complete setup: https://www.youtube.com/watch?v=Zbu71fWkYi0
