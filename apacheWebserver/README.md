# Apache webserver

I am currently using `podman` rather than `docker` due to my OS being fedora
and I find it better integrates with the OS, thanks RedHat.

But if you are using `docker` it should be the case of swapping `podman` to
`docker` on the commands below.

This document is to document how I use this current container when I'm developing
my static sites locally.
As I find it easy to test the website on my phone rather than a emulator through
the browser.

## How to build the image

To show the image in `podman images` you will need to run the following command
while in this current folder:

```shell
podman build -t webserver .
```

> From the selection of images I picked docker.io/library/httpd

## How to run the image

Once the container is built you can run the following command in the base of your
website dorectory structure:

```shell
podman run --name website -dit \
  -p 8080:80 \
  -v ${PWD}:/usr/local/apache2/htdocs/:Z \
  webserver
```

> Ensure you add :Z to your -v command else SELinux will give you permissions denied when accessing the website

Now in your browser you can type the following url:

- `localhost:8080`

If you want a custom url rather than `localhost` add the following into
`/etc/hosts` file:

``` shell
echo "127.0.0.1 DNS" >> /etc/hosts
```

## How to view the local server on your mobile

Now for the fun part of using this container is that you are able to browser the
web page locally on your mobile!

This makes like much easier when customising content in a mobile friendly manner.

First you will need to get the IP of your machine running the server with the following
`nmcli` command:

```shell
nmcli connection show CONN \
  | awk -F / '/IP4.ADDRESS/{print $1}' \
  | awk '{print $2}'
```

> Make sure you change the CONN to your current connection

From here you are then able to enter into the browser on your mobile:

- `IP:8080`

This will only work if your mobile is connected to the same network as your
computer.

## Podman systemd

To get the container to deploy on boot using systemd you will need to run the
following commands:

```shell
mkdir -p /home/${user}/.config/systemd/user/
cd /home/${user}/.config/systemd/user/
podman generate systemd --name webserver --new --files
systemd --user enable container-webserver
```

