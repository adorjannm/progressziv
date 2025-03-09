FROM node:23.5.0 AS builder

WORKDIR /app

COPY ./app/package.json ./app/package-lock.json ./

RUN npm install

COPY ./app .

RUN npm run build

# way smaller image
FROM node:23.5-alpine AS runner 

ENV MY_NAME="Adorján"

WORKDIR /app

COPY --from=builder /app/build /app/build

RUN npm install -g serve

EXPOSE 3000

CMD ["serve", "-s", "build"]
