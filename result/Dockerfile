FROM node:20-alpine AS base

# Install dependencies for healthcheck
RUN apk add --no-cache curl tini

WORKDIR /app

# Install dependencies in a separate layer
FROM base AS deps
COPY package.json ./
RUN npm install --omit=dev && npm cache clean --force

# Development stage
FROM base AS dev
RUN npm install -g nodemon
COPY package.json ./
RUN npm install
COPY . .
ENV PORT 4000
EXPOSE 4000
CMD ["nodemon", "server.js"]

# Production stage
FROM base AS production
COPY package.json ./
COPY --from=deps /app/node_modules ./node_modules
COPY . .

ENV PORT 4000
EXPOSE 4000

ENTRYPOINT ["/sbin/tini", "--"]
CMD ["node", "server.js"]
