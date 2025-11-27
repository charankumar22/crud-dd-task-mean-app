# backend/Dockerfile
FROM node:18-alpine

WORKDIR /usr/src/app

# Install dependencies
COPY package*.json ./
RUN npm install --only=production

# Copy source code
COPY . .

# Environment variables
ENV PORT=8080
ENV MONGO_URL=mongodb://mongo:27017/dd_db

EXPOSE 8080

CMD ["node", "server.js"]
