FROM debian:12.13

LABEL org.opencontainers.image.title="Testing Dockerfiles"
LABEL org.opencontainers.image.authoer="Ignacio Millan"

RUN apt update && apt upgrade -y

RUN echo "This command runs when I build the image. CHANGING"

WORKDIR /myapp

EXPOSE 8080

COPY hello.txt .

ENV MYVARIABLE=MYVALUE

ENTRYPOINT ["echo", "Hello from Entrypoint"]

CMD ["Hello from CMD"]


