
# ============================================
# Stage 1: Build React + Vite application
# ============================================
FROM node:24-bookworm-slim AS build

WORKDIR /app

# Copy dependency manifests first for Docker caching
COPY package.json package-lock.json ./

# Install exact dependencies from package-lock.json
RUN npm ci

# Copy application source code
COPY . .

# Build production application
RUN npm run build


# ============================================
# Stage 2: Production static server
# ============================================
FROM node:24-bookworm-slim AS production

WORKDIR /app

# Install lightweight static server
RUN npm install --global serve@14.2.4 \
    && npm cache clean --force

# Copy only production build files
COPY --from=build --chown=node:node /app/dist ./dist

# Run application as non-root user
USER node

# Application port
EXPOSE 4173

# Health check
HEALTHCHECK --interval=30s \
            --timeout=5s \
            --start-period=10s \
            --retries=3 \
    CMD wget -q -O /dev/null http://127.0.0.1:4173/ || exit 1

# Start static server
CMD ["serve", "-s", "dist", "-l", "tcp://0.0.0.0:4173"]
