---
title: setting up mariadb and phpmyadmin with podman
author: pienapin
date: 2024-11-25 21:06:00 +0700
categories: [guide]
tags: [mariadb, development environment, phpmyadmin, podman, technical]
toc: true
---

## The Why
---


## The Requirements
---
* podman

## The Steps
---
1. Install podman
    ```console
    paru -S podman
    # or
    sudo pacman -S podman
    ```
2. Create a pod
    ```console
    podman pod create --name pmardb -p 3306:3306 -p 8080:80
    ```
    A Pod in podman basically is a group of one or more containers that share the same network that allow them to communicate with each other directly through `localhost`.
    We create a pod because we are going to group two containers, mariadb container and phpmyadmin container. `--name` flag allows you to give name to the pod, while `-p` flag is to expose port from pod to the host machine (HostPort:PodPort). If we only need to run a web app with an exclusive database, we don't need to expose the database port which makes it safer. I expose it here so that i can access it later outside the pod.
3. Create a directory for mariadb volume
    ```console
    mkdir -p pod/mariadb
    ```
4. Run the mariadb container
    ```console
    podman run -d --pod pmardb --name mariadb \
    -e MYSQL_ROOT_PASSWORD=password \
    -e MYSQL_DATABASE=mardb \
    -e MYSQL_USER=user \
    -e MYSQL_PASSWORD=password \
    -v /home/user/pod/mariadb:/var/lib/mysql \
    --restart=always \
    docker.io/library/mariadb
    ```
    `podman run` is a command to run podman container. `-d` flag stands for detached mode, it allows the container to run in the background. `--pod` is a flag that is used to specify the pod that the container will be in. `--name` is to specify the container name and `-e` is a flag to set an environment variable. A `-v` flag is to mount a volume from the host to the container. `--restart=always` is a flag that sets the container to automatically restart the container if it stops or the host reboot. `docker.io/library/mariadb` is an image that will be used in the container.
5. Run the phpmyadmin container
    ```console
    podman run -d --pod pmardb \
    --name phpmyadmin \
    -e PMA_HOST=127.0.0.1 \
    -e PMA_PORT=3306 \
    docker.io/library/phpmyadmin
    ```

And that's it! Now you can access your mariadb database through 127.0.0.1:3306 or through phpmyadmin in 127.0.0.1:8080. Since im running it in a VM, i can open the phpmyadmin in the browser outside the VM through 192.168.107.119:8080 which is my VM local IP.
