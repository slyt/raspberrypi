# Raspberry Pi Notes

These are notes that I use when working with raspberry pi.

## Networking

Note that `ifconfig` is deprecated, use `ip` command instead. For example, to get a list of networking interfaces and their ip addresses, run `ip address`.

Discovering/scanning for WIFI networks with
`sudo iwlist scan`. `sudo` is necessary, otherwise you will get a cached scan result.


Add a Wifi network credentials by editing `/etc/wpa_supplicant/wpa_suppicant.conf`. Note that this is NOT JSON format, so don't put commas before new lines.:
```
network={
    ssid="Some WIFI SSID here"
    psk="The wifi password"
}

network={
    ssid="Another WIFI SSID here"
    psk="The other wifi network password"
}
```

After editing `wpa_supplicant.conf` you'll need to restart the networking service with `sudo systemctl restart networking.service`. Alternatively, run `sudo ifdown wlan0` and then `sudo ifup wlan0`

## Docker

_TODO: Add docker install instructions_

[linux-postinstall](https://docs.docker.com/engine/install/linux-postinstall/) instrucitons.

In order to execute `docker` commands without invoking `sudo`, need to add the current user to the docker group:
```bash
sudo groupadd docker           # create docker group if it doesn't exist already
sudo usermod -aG docker $USER  # add the current user to docker group
newgrp docker                  # Refresh docker group so updates become live
docker run hello-world         # verify by running docker hello-world image
```

## Github access
In order to clone GitHub repos to the raspberry pi, an SSH key pair needs to be generate, then the public key needs to be added to your GitHub account.

Generate SSH key pair:
```
$ ssh-keygen
Enter file in which to save the key (/home/pi.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/pi/.ssh/id_rsa.
Your public key has been saved in /home/pi/.ssh/id_rsa.pub.
They key fingerprint is:
<REDACTED>
```

Now `cat` your public key and copy/paste it into GitHub under Settings -> SSH and GPG keys -> New SSH key (https://github.com/settings/ssh/new):
```
$ cat /home/pi/.ssh/id_rsa.pub
```

## Java install
```
$ sudo apt update
$ sudo apt install default-jdk
```

## Sheepit Render Farm
Sheepit is a way to earn rendering points to render Blender projects.


_TODO: I tried the ARM based java implementation below. Installed java, installed ant, and ran `ant` in the root directory. However, executing the `./bin/sheepit-client.jar` that was built with ant shows an error saying that the SheepIt client needed to be updated. Will back-burner this for later._

This ARM based alternative of SheeptIt may work for raspberrypi: https://github.com/TheGreatRambler/sheepit-client-ARM


_TODO: The image noted below does not work on linux/arm/v7, thus does not work on raspberry pi_

There is a dockerized version of sheepit [AGSPhoenix/sheepit-docker](https://github.com/AGSPhoenix/sheepit-docker)

To simplify running the command, a bash script can be made:
```bash
docker run -d \
 --name "sheepit" \
 -e user_name=XXXXXX \
 -e user_password=XXXXXX \
 agsphoenix/sheepit-docker:latest
 ```

 Don't forget to change the script permissiosn to make it executable: `chmod u+x start_sheepit.sh`. Using `u+x` makes a file executable by the current user .`+x` makes it executable by everyone, which may be a security concern.