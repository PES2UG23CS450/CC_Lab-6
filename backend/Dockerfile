FROM alpine:3.19

RUN apk add --no-cache g++

WORKDIR /app

COPY . .

RUN g++ backend.cpp -o backend

CMD ["./backend"]
