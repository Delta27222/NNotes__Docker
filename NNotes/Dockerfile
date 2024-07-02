# # Dependencias de desarrollo
# FROM node:19.2-alpine3.16 as deps
# WORKDIR /app
# COPY package.json ./
# RUN npm install

# # Build y Tests
# FROM node:19.2-alpine3.16 as builder
# WORKDIR /app
# COPY --from=deps /app/node_modules ./node_modules
# COPY . .
# RUN npm run test

# # Dependencias de Producción
# FROM node:19.2-alpine3.16 as prod-deps
# WORKDIR /app
# COPY package.json ./
# RUN npm install --prod

# # Ejecutar la APP
# FROM node:19-alpine AS prod
# WORKDIR /app
# EXPOSE 3000
# ENV NODE_ENV=production
# COPY --from=prod-deps /app/node_modules ./node_modules
# COPY --from=builder /app/.next ./
# CMD [ "npm", "start" ]  # Use npm start for Next.js development server


FROM node:18-alpine AS build

WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:18-alpine AS runtime

WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY --from=build /app/.next ./.next
COPY --from=build /app/public ./public

EXPOSE 3000
USER node
CMD ["npm", "start"]