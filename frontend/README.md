# Frontend - ReactJS with ChakraUI

This directory contains the frontend of the application built with ReactJS and ChakraUI.

## Prerequisites

- Node.js (version 14.x or higher)
- npm (version 6.x or higher)

## Setup Instructions

1. **Navigate to the frontend directory**:
    ```sh
    cd frontend
    ```

2. **Install dependencies**:
    ```sh
    npm install
    ```

3. **Run the development server**:
    ```sh
    npm run dev
    ```

4. **Configure API URL**:
   Ensure the API URL is correctly set in the `.env` file.
REACT_APP_API_URL=http://localhost
Additional Scripts
5. **Build the Application**:
npm run build
This command builds the production-ready application in the build folder.

6. **Run Tests**:
npm test

Dockerization
Dockerize the React Frontend:

Navigate to the React frontend directory.
Create a Dockerfile with the following content:
Dockerfile
# Stage 1: Build the React app
FROM node:14 AS build
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
RUN npm run build

# Stage 2: Serve the React app with Nginx
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

# Build and run the React frontend container:
docker build -t react-frontend .
docker run -d -p 3000:80 react-frontend


Execute tests using the configured test runner.

Folder Structure
The main folders and files within the frontend directory are structured as follows:

src/: Contains the application source code.
public/: Contains static assets and the index.html file.
Deployment
For deployment, ensure to configure appropriate environment variables and build the application using npm run build. Serve the built files using a server capable of handling a ReactJS application.

Troubleshooting
If encountering issues, ensure dependencies are installed correctly and environment variables are configured properly.
Check the console for any errors or warnings that might provide clues on how to resolve issues.

