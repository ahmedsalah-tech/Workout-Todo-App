# Workout Todo - MERN Stack

This is a full-stack workout tracking application built with the MERN stack (MongoDB, Express, React, Node.js).

## Project Structure

The project is a monorepo with two main directories:

- `frontend`: A React single-page application for the user interface.
- `backend`: An Express.js server that provides a REST API for managing workouts and users.

## Getting Started

### Prerequisites

- Node.js and npm
- MongoDB

### Installation and Setup

1. **Clone the repository:**

    ```bash
    git clone https://github.com/ahmedsalah-tech/Workout-Todo-App.git
    cd Workout-Todo--MERN
    ```


2. **Install dependencies for both frontend and backend:**

    ```bash
    npm install
    cd frontend
    npm install
    cd ../backend
    npm install
    ```


3. **Set up environment variables:**

    In the `backend` directory, you will find a file named `.env.example`. This file contains the template for the environment variables needed to run the backend server.

    Create a new file named `.env` in the `backend` directory and copy the contents of `.env.example` into it.

    ```bash
    backend/.env
    ```

    Update the `.env` file with your own secret values:

    ```ini
    PORT=4000
    MONGO_URI=<your_mongodb_connection_string>
    SECRET=<your_jwt_secret>
    ```

    **Important**: The `.env` file contains sensitive information and should **not** be committed to version control.

## Development Workflow

To run both the frontend and backend servers concurrently for development, use the following command from the project root directory:

```bash
npm run dev
```

- The backend server will start on `http://localhost:4000` and will automatically restart on file changes using `nodemon`.
- The frontend development server will start on `http://localhost:3000`.
