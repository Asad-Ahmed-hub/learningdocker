# Use the official Node.js 22 image as the base
FROM node:22-bullseye-slim

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json to the container
COPY package*.json ./

# Install project dependencies
RUN npm install

# Copy the rest of the application code to the container
COPY . .

# Expose the ports used by Expo
EXPOSE 19000 19001 19002

# Start the Expo development server using npx
CMD ["npx", "expo", "start", "--host", "lan"]
