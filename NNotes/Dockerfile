# Stage 1: Install dependencies (development)
FROM node:19-alpine3.15 AS dev-deps
WORKDIR /app
COPY package.json ./
RUN yarn install --frozen-lockfile

# Stage 2: Build the application (production)
FROM node:19-alpine3.15 AS builder
WORKDIR /app
COPY --from=dev-deps /app/node_modules ./node_modules
COPY . .
RUN yarn build

# Stage 3: Install production dependencies
FROM node:19-alpine3.15 AS prod-deps
WORKDIR /app
COPY package.json ./
RUN yarn install --prod --frozen-lockfile

# Stage 4: Final production image
FROM node:19-alpine3.15 AS prod
WORKDIR /app
EXPOSE 3000
ENV NODE_ENV=production
COPY --from=prod-deps /app/node_modules ./node_modules
COPY --from=builder /app/.next ./
CMD [ "npm", "start" ]
