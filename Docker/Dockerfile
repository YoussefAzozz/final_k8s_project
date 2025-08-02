
FROM node:22-slim as builder
WORKDIR /app
COPY package*.json ./
RUN npm install && npm cache clean --force
COPY . .
RUN npm run build


FROM node:18-alpine

WORKDIR /app

COPY --from=builder /app .

# Create a non-root user and group to run the application
RUN addgroup -S appgroup && adduser -S appuser -G appgroup &&  \
    chown -R appuser:appgroup /app


USER appuser

EXPOSE 5000

ENTRYPOINT ["node"]

CMD ["index.js"]
