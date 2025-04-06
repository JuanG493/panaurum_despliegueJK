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
    python3 \
    nginx  # Add nginx to serve the built files

# Clone repository using HTTPS with token
ARG GITHUB_TOKEN
ARG FRONTEND_REPO_URL
ARG API_URL

# Clone repository
RUN git clone --depth=1 https://${GITHUB_TOKEN}@github.com/${FRONTEND_REPO_URL}.git /app || \
    (cd /app && git fetch origin && git reset --hard origin/main)

WORKDIR /app

# Set environment variables for the build
# This will create a .env file with the API URL
RUN echo "VITE_API_URL=${API_URL}" > .env

# Install and build
RUN npm install --force && npm run build

# Configure nginx to serve the application
RUN mkdir -p /var/log/nginx
COPY frontend/nginx-container.conf /etc/nginx/nginx.conf

# Expose port
EXPOSE 80

# Start nginx to serve the built files
CMD ["nginx", "-g", "daemon off;"]
