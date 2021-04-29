FROM nginx:1.18.0
LABEL maintainer="George Dimosthenous"

# Set working directory
WORKDIR /var/www

ARG UID
ARG GID

# Install dependencies
RUN apt-get update && apt-get install -y \
    apt-utils \
    procps 

RUN usermod -u $UID nginx
RUN groupmod -g $GID nginx