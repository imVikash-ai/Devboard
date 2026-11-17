# Build the Vite app.
FROM dhi.io/node:22-alpine3.24-dev AS build

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci --legacy-peer-deps

COPY . .
RUN npm run build

# Serve only static assets; do not include Node.js dependencies in runtime.
FROM dhi.io/nginx:1.30.6-alpine3.24 AS runtime

COPY --from=build --chown=nginx:nginx /app/dist /usr/share/nginx/html

USER nginx
EXPOSE 8080
