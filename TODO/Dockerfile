# Use Node base image
FROM node:18

# Create app directory
WORKDIR /app

# Copy backend
COPY todo_backend ./todo_backend

# Copy frontend
COPY todo_frontend ./todo_frontend

# Install backend dependencies
WORKDIR /app/todo_backend
RUN npm install

# Install frontend dependencies
WORKDIR /app/todo_frontend
RUN npm install
RUN npm run build

# Expose backend port
EXPOSE 5000

# Start backend server
WORKDIR /app/todo_backend
CMD ["npm", "start"]