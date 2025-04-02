FROM node:20-alpine

# Install required dependencies
RUN apk add --no-cache \
    git \
    autoconf \
    automake \
    libtool \
    make \
    g++ \
    pkgconfig \
    zlib \
    zlib-dev \
    libpng \
    libpng-dev \
    nasm \
    python3

# Clone repository using HTTPS with token
ARG GITHUB_TOKEN
ARG FRONTEND_REPO_URL
ARG API_URL

# Clone repository
RUN git clone --depth=1 https://${GITHUB_TOKEN}@github.com/${FRONTEND_REPO_URL}.git /app

WORKDIR /app

# Set environment variables for the build
# This will create a .env file with the API URL
RUN echo "VITE_API_URL=${API_URL}" > .env

# Install and build
RUN npm install --force && npm run build

# Expose port
EXPOSE 5173

# Start server - use the appropriate command for your frontend framework
# For Vite-based projects:
CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]
