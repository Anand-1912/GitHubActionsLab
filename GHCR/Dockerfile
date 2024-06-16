FROM alpine:3.10

RUN apk add --no-cache bash

WORKDIR /app

COPY entrypoint.sh .

RUN chmod +x entrypoint.sh

ENTRYPOINT [ "/app/entrypoint.sh" ]