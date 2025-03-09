FROM node:23.5.0 AS builder

WORKDIR /app

COPY ./Simple-Calulator/package.json ./Simple-Calulator/package-lock.json ./

RUN npm install

COPY ./Simple-Calulator .

RUN npm run build

# way smaller image
FROM node:23.5-alpine AS runner 

WORKDIR /app

COPY --from=builder /app/build /app/build

RUN npm install -g serve

EXPOSE 3000

CMD ["serve", "-s", "build"]
