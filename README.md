# Why??

### 自己建的環境，自己就是神！！！

You are root by default in this environment! Which means less work for ITs 😇

Also, the possibility of using software that is newer than the base system.

# Rootless docker distrobox setup guide

## [**Prerequisites**](https://docs.docker.com/engine/security/rootless/#prerequisites)

- You must install `newuidmap` and `newgidmap` on the host. These commands are provided by the `uidmap` package on most distros.
- `/etc/subuid` and `/etc/subgid` should contain at least 65,536 subordinate UIDs/GIDs for the user. In the following example, the user `testuser` has 65,536 subordinate UIDs/GIDs (231072-296607).

## [Install](https://docs.docker.com/engine/security/rootless/#install)

> **Note**
> 
> 
> If the system-wide Docker daemon is already running, consider disabling it:
> 
> `$ sudo systemctl disable --now docker.service docker.socket
> $ sudo rm /var/run/docker.sock`
> 
> Should you choose not to shut down the `docker` service and socket, you will need to use the `--force` parameter in the next section. There are no known issues, but until you shutdown and disable you're still running rootful Docker.
> 

---

If you installed Docker 20.10 or later with [RPM/DEB packages](https://docs.docker.com/engine/install), you should have `dockerd-rootless-setuptool.sh` in `/usr/bin`.

Run `dockerd-rootless-setuptool.sh install` as a non-root user to set up the daemon:

```bash
$ dockerd-rootless-setuptool.sh install
[INFO] Creating /home/testuser/.config/systemd/user/docker.service
...
[INFO] Installed docker.service successfully.
[INFO] To control docker.service, run: `systemctl --user (start|stop|restart) docker.service`
[INFO] To run docker.service on system startup, run: `sudo loginctl enable-linger testuser`

[INFO] Make sure the following environment variables are set (or add them to ~/.bashrc):

export PATH=/usr/bin:$PATH
export DOCKER_HOST=unix:///run/user/1000/docker.sock
```

If `dockerd-rootless-setuptool.sh` is not present, you may need to install the `docker-ce-rootless-extras` package manually, e.g.,

`$ sudo apt-get install -y docker-ce-rootless-extras`

---

See [Troubleshooting](https://docs.docker.com/engine/security/rootless/#troubleshooting) if you faced an error.

# Installation

There are two options:
1. Download the original distrobox
  
    Please refer to the Distrobox [installation page](https://distrobox.it/#installation). 
    You also have to download the patch file at [distrobox-rootless.patch](https://raw.githubusercontent.com/CS-EVA-LAB/distrobox-rootless-docker/master/distrobox-rootless.patch).

    After downloading, run the patch command to patch the executables:

    ```
    cd ~/.local/bin
    patch < distrobox-rootless.patch
    ```

2.  Use the version provided in this repo

    Simply git clone this repo. It already works ot of the box 🙃


# Usage

For more usage please reference the official docs at [distrobox.it](https://distrobox.it/).
We only show you some useful examples:

## Create environment

```
distrobox create --image ubuntu:24.04 --name "YOUR CONTAINER NAME" --hostname "YOUR HOSTNAME" --nvidia --volume "SRC_ON_HOST:TARGET_PATH_IN_CONTAINER"
```

- `--nvidia` is required for nvidia integration with docker.

- `--volume` is a flag for mounting additional folders inside the container.


## Entering the environment

```
distrobox enter "YOUR_CONTAINER_NAME"
```


> ### **⚠️ Note ⚠️**
>If you plan on continue running the docker container after you log out, and you don't want systemd to kill all processes under `user*.slice`, there are two options:
>   1. leave a tmux session open on the host (since there is at least one user logged in the system, systemd won't kill the user session).
>   2. use `loginctl enable-linger` on the current user.


# Enjoy being **root/God** !