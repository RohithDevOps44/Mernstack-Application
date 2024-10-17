# Stage 1: Build stage using Node.js image
FROM node:18.9.1 AS build

WORKDIR /app

# Copy package.json and install dependencies
COPY package.json .

RUN npm install

# Copy all files to the container
COPY . .

# Stage 2: Production stage using Distroless image
FROM gcr.io/distroless/nodejs18-debian12

WORKDIR /app

# Copy only the necessary files from the build stage
COPY --from=build /app .

EXPOSE 5173

# Run the Vite development server directly
CMD ["node_modules/.bin/vite", "--host", "0.0.0.0"]

