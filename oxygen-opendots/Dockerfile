FROM node:24-bookworm-slim AS build
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends git ca-certificates && rm -rf /var/lib/apt/lists/*
RUN git clone https://github.com/CopilotKit/OpenDots.git .
RUN npm ci
RUN npm run build

FROM node:24-bookworm-slim AS app
ENV NODE_ENV=production HOST=0.0.0.0 PORT=4310 DATABASE_PATH=/data/opendots.sqlite
WORKDIR /app
COPY --from=build /app/package*.json ./
RUN npm ci --omit=dev && mkdir -p /data && chown node:node /data
COPY --from=build /app/dist ./dist
USER node
EXPOSE 4310
CMD ["node", "dist/server/server/index.js"]
