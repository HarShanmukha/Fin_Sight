FROM node:20-alpine AS builder
RUN apk add --no-cache git
WORKDIR /app
COPY package.json pnpm-lock.yaml ./
RUN npm install -g pnpm && pnpm install --no-frozen-lockfile
COPY . .
ARG VITE_API_URL
ARG VITE_WS_URL
ENV VITE_API_URL=$VITE_API_URL
ENV VITE_WS_URL=$VITE_WS_URL
RUN pnpm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
RUN printf 'server {\n  listen 3000;\n  root /usr/share/nginx/html;\n  location / { try_files $uri /index.html; }\n}\n' > /etc/nginx/conf.d/default.conf
EXPOSE 3000
