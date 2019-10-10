# SSHD-Borg

Minimal Alpine Linux Docker image with `sshd` exposed and `borg backup` installed.

Mount the space for your Borg repositories at `/borg` and set `BORG_USERS` environment variable to create user accounts (see below).

Optionally mount a custom sshd config at `/etc/ssh/`. You can also mount the host keys at `/etc/ssh/keys` so they don't change at each run (see below).

## Environment Options

- `BORG_USERS` list of user accounts with public keys. e.g. `BORG_USERS='foo:ssh-rsa:LONGSSHPUBKEY:foo@foo.net bar:ssh-rsa:ANOTHERPUBSSHKEY:bar@bar.net'`
- `MOTD` change the login message
- `GATEWAY_PORTS` if "true" sshd will allow gateway ports (port forwardings not bound to the loopback address)

## SSH Host Keys

SSH uses host keys to identity the server you are connecting to. To avoid receiving security warning the containers host keys should be mounted on an external volume.

By default this image will create new host keys in `/etc/ssh/keys` which should be mounted on an external volume. If you are using existing keys and they are mounted in `/etc/ssh` this image will use the default host key location making this image compatible with existing setups.

If you wish to configure SSH entirely with environment variables it is suggested that you externally mount `/etc/ssh/keys` instead of `/etc/ssh`.

## Usage Example

```
docker run -d -p 2222:22 --mount type=bind,source=/share/borg,target=/borg --mount type=bind,source=/path/to/keys,target=/etc/ssh/keys -e BORG_USERS='jamestkirk:ssh-rsa:AAAAB3Nza......EWg4E0w==:jim@enterprise' docker.io/vajsravana/sshd-borg:0.1
```

## Acknowledgements and thanks

Thanks go to the author of the image, exposing ssh and rsync, by [Panubo](https://github.com/panubo), who did most of the work. I added borg-backup, remove some features and modified the user creation part. You can find the original work at https://github.com/panubo/docker-sshd

## Status

Under development right now! Use at your own risk!


