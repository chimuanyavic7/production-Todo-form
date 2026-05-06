# Stage 1: Build React app
FROM node:20-alpine AS build

# Set working directory
WORKDIR /app

# Copy only package.json for better caching
COPY package.json ./

# Install dependencies
RUN npm install

# Copy the rest of the source code
COPY . ./

# Fix Node 20 OpenSSL issue
ENV NODE_OPTIONS=--openssl-legacy-provider

# Build the app
RUN npm run build

# Stage 2: Serve with Nginx
FROM nginx:1.27-alpine AS production
RUN apk update && apk upgrade

# Remove default Nginx content
RUN rm -rf /usr/share/nginx/html/*

# Copy built React app from build stage
COPY --from=build /app/build /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]