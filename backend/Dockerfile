FROM public.ecr.aws/docker/library/ubuntu:22.04

RUN apt-get update && apt-get install -y g++

WORKDIR /app

COPY app.cpp .

RUN g++ app.cpp -o app

CMD ["./app"]
