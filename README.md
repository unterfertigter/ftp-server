# FTP Server

Minimalistic Docker image for a FTP server to run inside an Alpine Linux LXC container running inside Proxmox.

The FTP server is using [vsftpd](https://security.appspot.com/vsftpd.html). This Docker image is basically an enhanced version of [garethflowers/ftp-server](https://hub.docker.com/r/garethflowers/ftp-server), see [garethflowers/docker-ftp-server](https://github.com/garethflowers/docker-ftp-server).

Running the original image [garethflowers/ftp-server](https://hub.docker.com/r/garethflowers/ftp-server) inside a LXC container in Promox, no passive FTP mode is possible because of the wrong IP address  `pasv_address=0.0.0.0` in `/etc/vsftpd.conf`. The additional environment parameter `HOST_IP` fixes that, it has to be set to the local IP of the host the docker container runs on.

## How to start a FTP server instance

To start a container with data stored in `/ftp` on the host:

```sh
docker run \
 --restart=always \
 --detach \
 --env HOST_IP=192.168.123.123 \
 --env FTP_PASS=password \
 --env FTP_USER=username \
 --name my-ftp-server \
 --publish 20-21:20-21/tcp \
 --publish 40000-40009:40000-40009/tcp \
 --volume /ftp:/home/username \
 unterfertigter/ftp-server
```

## License

This image is released under the [MIT License](https://raw.githubusercontent.com/garethflowers/docker-ftp-server/master/LICENSE).
