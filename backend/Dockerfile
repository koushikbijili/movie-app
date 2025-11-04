# Use Node.js official image
FROM node:18

# Set working directory
WORKDIR /app

# Copy package files first
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy app source
COPY . .

# Expose backend port
EXPOSE 5000

# Start backend
CMD ["node", "server.js"]


