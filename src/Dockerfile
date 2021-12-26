FROM node:17 AS build
WORKDIR /app
COPY ["package.json", "package-lock.json*", "./"]
RUN npm install --production
FROM node:17-alpine AS deploy
COPY --from=build /app/node_modules ./node_modules
COPY . .
EXPOSE 8080
CMD [ "node", "server.js" ]
