# Use the official image as a parent image
FROM node:18.9.1 AS build

# Set the working directory
WORKDIR /app

# Copy the file from your host to your current location
COPY package.json .

# Run the command inside your image filesystem
RUN npm install

COPY . .

# Stage 2: Production stage using Distroless image
FROM node:alpine3.20

WORKDIR /app

# Copy only the necessary files from the build stage
COPY --from=build /app .

EXPOSE 5050

CMD ["npm", "start"]

