# A2A Inspector (Multi-Version)

A fork of [a2a-inspector](https://github.com/a2aproject/a2a-inspector) with **explicit protocol version selector**, supporting both **A2A v0.3 (Legacy)** and **v1.0 (Latest)**.

> The official inspector is stuck at v0.3 and PR #145 (v1.0 migration) was never merged. This fork merges that PR and adds a user-facing version selector.

## What's Different

- **Protocol Version Selector** — Choose `v0.3`, `v1.0`, or `Auto Detect` from the UI dropdown before connecting
- **Backend Version Routing** — The server uses the selected version to call the correct SDK code paths (no more silent try/except fallbacks)
- **Dual-Format Validation** — Agent cards and messages are validated against the correct schema based on version

## Features

- **Connect to any A2A Agent:** Specify the agent card URL to connect (e.g., `http://localhost:5555`)
- **Protocol Version Selector:** Pick v0.3, v1.0, or let the inspector auto-detect
- **View Agent Card:** Automatically fetches and displays the agent's card
- **Spec Compliance Checks:** Validates agent cards and messages against the A2A specification
- **Live Chat:** Send and receive messages with the connected agent
- **Debug Console:** Raw JSON-RPC 2.0 messages between inspector and agent

## Prerequisites

- Python 3.10+
- [uv](https://github.com/astral-sh/uv)
- Node.js and npm

## Project Structure

This repository is organized into two main parts:

- `./backend/`: Contains the Python FastAPI server that handles WebSocket connections and communication with the A2A agent.
- `./frontend/`: Contains the TypeScript and CSS source files for the web interface.

## Setup and Running the Application

Follow these steps to get the A2A Inspector running on your local machine. The setup is a three-step process: install Python dependencies, install Node.js dependencies, and then run the two processes.

### 1. Clone the repository

```sh
git clone https://github.com/2862282695gjh-afk/a2a-inspector-for-all-version.git
cd a2a-inspector-for-all-version
```

### 2. Install Dependencies

First, install the Python dependencies for the backend from the root directory. `uv sync` reads the `uv.lock` file and installs the exact versions of the packages into a virtual environment.

```sh
# Run from the root of the project
uv sync
```

Next, install the Node.js dependencies for the frontend.

```sh
# Navigate to the frontend directory
cd frontend

# Install npm packages
npm install

# Go back to the root directory
cd ..
```

### 3. Run the Application

You can run the A2A Inspector in two ways. Choose the option that best fits your workflow:

- Option 1 (Run Locally): Best for developers who are actively modifying the code. This method uses two separate terminal processes and provides live-reloading for both the frontend and backend.
- Option 2 (Run with Docker): Best for quickly running the application without managing local Python and Node.js environments. Docker encapsulates all dependencies into a single container.

#### Option 1: Run Locally

This approach requires you to run two processes concurrently. You can either use the provided convenience script or run them separately in different terminals.

**Using the convenience script (recommended):**

```sh
# Make the script executable (first time only)
chmod +x scripts/run.sh

# Run both frontend and backend with a single command
bash scripts/run.sh
```

This will start both the frontend build process and backend server, displaying their outputs with colored prefixes. Press `Ctrl+C` to stop both services.

**Or manually in separate terminals:**

Make sure you are in the root directory of the project (`a2a-inspector`) before starting.

**In your first terminal**, run the frontend development server. This will build the assets and automatically rebuild them when you make changes.

```sh
# Navigate to the frontend directory
cd frontend

# Build the frontend and watch for changes
npm run build -- --watch
```

**In a second terminal**, run the backend Python server.

```sh
# Navigate to the backend directory
cd backend

# Run the FastAPI server with live reload
uv run app.py
```

##### Access the Inspector

Once both processes are running, open your web browser and navigate to:
**[http://127.0.0.1:5001](http://127.0.0.1:5001)**

#### Option Two: Run with Docker

This approach builds the entire application into a single Docker image and runs it as a container. This is the simplest way to run the inspector if you have Docker installed and don't need to modify the code.

From the root directory of the project, run the following command. This will build the frontend, copy the results into the backend, and package everything into an image named a2a-inspector.

```sh
docker build -t a2a-inspector .
```

Once the image is built, run it as a container.

```sh
# It will run the container in detached mode (in the background)
docker run -d -p 8080:8080 a2a-inspector
```

The container is now running in the background. Open your web browser and navigate to:
**[http://127.0.0.1:8080](http://127.0.0.1:8080)**

### 4. Inspect your agents

- Try inputting a sample agent URL such as `https://sample-a2a-agent-908687846511.us-central1.run.app`
