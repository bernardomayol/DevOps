FROM node:16.13.2-alpine AS build

ENV NODE_ENV=production

WORKDIR /home/ansible_controller/hello-world

COPY ["package.json", "package-lock.json*", "./"]

RUN npm install --production

COPY . .

RUN npm run build 

FROM ubuntu/nginx:1.18-21.10_beta AS final

WORKDIR /usr/share/nginx/html

COPY --from=build /home/ansible_controller/hello-world/build .

RUN apt-get update  -y \
    && apt-get upgrade -y \ 
    && apt-get install -y \
    ssh \
    sshpass \
    sudo \
    software-properties-common

RUN useradd -rm -d /home/ansible_controller/hello-world -s /bin/bash -g root -G sudo -u 1001 ansible_controller
RUN echo ansible_controller:12345 | chpasswd
RUN echo "ansible_controller ALL=(ALL:ALL) NOPASSWD: ALL" |  EDITOR="tee -a"  visudo

RUN mkdir -p /home/ansible_controller/hello-world/.ssh

CMD ["/bin/bash", "/home/ansible_controller/hello-world/ansible_base/startup.sh"]