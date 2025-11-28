FROM node:18-alpine

WORKDIR /app

# Copy only package.json
COPY package.json ./

# Install dependencies
RUN npm install

# Copy rest of backend source
COPY . .

EXPOSE 8080

CMD ["node", "server.js"]
