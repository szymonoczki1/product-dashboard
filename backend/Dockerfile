# Stage 1 — install production dependencies only
FROM node:20-alpine AS builder

WORKDIR /app
COPY package*.json ./
RUN npm ci --omit=dev && npm cache clean --force

# Stage 2 — runtime image (no build tools, smaller layer)
FROM node:20-alpine AS runtime

ARG IMAGE_VERSION
LABEL version=$IMAGE_VERSION

ENV NODE_ENV=production PORT=3000

WORKDIR /app

COPY --from=builder /app/node_modules ./node_modules
COPY . .

EXPOSE 3000
USER node

CMD ["node", "server.js"]
