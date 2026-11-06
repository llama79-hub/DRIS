# ==========================================
# Frontend image — build the React app, then
# serve it (and proxy /api to the backend) with nginx
# ==========================================

# ---- Stage 1: build ----
FROM node:20-slim AS build
WORKDIR /app

COPY package.json package-lock.json* ./
RUN npm install

COPY . .

# Baked in at build time — the built JS will call this path for every
# API request. "/api" matches nginx.conf's proxy_pass rule below.
ENV VITE_API_BASE=/api
RUN npm run build

# ---- Stage 2: serve ----
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
