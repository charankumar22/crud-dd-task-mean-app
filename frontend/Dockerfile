# ---- Build stage ----
FROM node:18-alpine AS build

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install

COPY . .

# Production build
RUN npm run build -- --configuration production

# ---- Run stage ----
FROM nginx:1.27-alpine

# Copy Angular build output to Nginx html directory
COPY --from=build /usr/src/app/dist/angular-15-crud /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
