###########################
#####       Base     ######
###########################
FROM node:14.13.1

LABEL RUN="podman run -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY -v $(pwd)/src:/app/src --rm -it electron-wrapper bash"

RUN apt-get update && apt-get install \
    git libx11-xcb1 libxcb-dri3-0 libxtst6 libnss3 libatk-bridge2.0-0 libgtk-3-0 libxss1 libasound2 libcanberra-gtk-module libcanberra-gtk3-module \
    -yq --no-install-suggests --no-install-recommends && apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY . .
RUN chown -R node /app

USER node
RUN npm install
RUN npx electron-rebuild

# Electron needs root for sand boxing
# see https://github.com/electron/electron/issues/17972
USER root
RUN chown root /app/node_modules/electron/dist/chrome-sandbox
RUN chmod 4755 /app/node_modules/electron/dist/chrome-sandbox

# Electron doesn't like to run as root
USER node
CMD bash

# ###########################
# #####      Nginx     ######
# ###########################

# FROM nginx:alpine
# COPY --from=built_app /usr/src/app/build /var/www
# RUN rm /etc/nginx/conf.d/default.conf
# COPY nginx/default.conf /etc/nginx/conf.d/default.conf
# EXPOSE 4780 443
# CMD ["nginx", "-g", "daemon off;"]
