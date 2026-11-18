# Stage 1: Install production dependencies only
FROM node:26-alpine AS builder
WORKDIR /usr/src/app
COPY package.json ./
RUN npm install --omit=dev

# Stage 2: Minimal, non-root runtime image
FROM node:26-alpine
WORKDIR /usr/src/app

# Copy dependencies and source with non-root ownership
COPY --from=builder --chown=node:node /usr/src/app/node_modules ./node_modules
COPY --chown=node:node package.json server.js ./

# Switch to built-in non-root user for container security
USER node

EXPOSE 3000
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD wget -qO- http://localhost:3000/health || exit 1

CMD ["node", "server.js"]