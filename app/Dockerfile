# Use the latest stable version of Alpine Linux
FROM alpine:3.18

ENV TERM=linux

RUN echo 'http://dl-cdn.alpinelinux.org/alpine/v3.6/main' >> /etc/apk/repositories
RUN echo 'http://dl-cdn.alpinelinux.org/alpine/v3.6/community' >> /etc/apk/repositories

RUN apk update && apk --no-cache --update add mongodb

# Create the MongoDB data directory and set the appropriate ownership
RUN mkdir -p /data/db && \
    chown -R root:root /data/db

# Define a volume for MongoDB data to persist
VOLUME /data/db

# Expose MongoDB port to allow external connections
EXPOSE 27017

# Start the MongoDB server and bind it to all network interfaces
CMD [ "mongod", "--bind_ip", "0.0.0.0"]
