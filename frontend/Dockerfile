# Stage 1: Build the React application
FROM node:14 AS build

WORKDIR /app

COPY package.json package-lock.json ./

# Install Git
RUN apt-get update && apt-get install -y git

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the application
RUN npm run build

# Debugging: List the contents of the /app directory
RUN ls -al /app

# Debugging: List the contents of the /app/dist directory
RUN ls -al /app/dist

# Stage 2: Serve the application with nginx
FROM nginx:alpines

# Copy the build output from the previous stage
COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
