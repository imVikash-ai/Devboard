# Build the Vite frontend.
FROM dhi.io/node:26-alpine3.24-dev AS build

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .
RUN npm run build

# Serve only the generated static files.
FROM dhi.io/nginx:1.30.6-alpine3.24 AS runtime

COPY --from=build --chown=nginx:nginx /app/dist /usr/share/nginx/html

USER nginx
EXPOSE 8080
