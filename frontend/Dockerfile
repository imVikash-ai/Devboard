# Build the app and prepare Vite for the preview runtime.
FROM dhi.io/node:22-alpine3.24-dev AS build

WORKDIR /app

COPY package*.json ./
RUN npm install --legacy-peer-deps

COPY . .
RUN npm run build

# Install only Vite for the runtime stage.
RUN npm install --prefix /vite-runtime \
    --omit=dev \
    --legacy-peer-deps \
    vite@5

# Serve the built files with Vite's preview server.
FROM dhi.io/node:22-alpine3.24 AS runtime

WORKDIR /app

COPY --from=build --chown=65532:65532 /app/dist ./dist
COPY --from=build --chown=65532:65532 /vite-runtime/node_modules ./node_modules
COPY --chown=65532:65532 vite.preview.config.js ./vite.config.js

USER 65532:65532
EXPOSE 4173

ENTRYPOINT ["node", "/app/node_modules/vite/bin/vite.js"]
CMD ["preview", "--host", "0.0.0.0", "--port", "4173"]
