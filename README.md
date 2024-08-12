# Why??

### 自己建的環境，自己就是神！！！

You are root by default in this environment! Which means less work for ITs 😇

Also, the possibility of using software that is newer than the base system.

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

### Enjoy being **root/God** !