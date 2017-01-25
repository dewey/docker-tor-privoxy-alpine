# tor-privoxy-alpine

[![](https://badge.imagelayers.io/rdsubhas/tor-privoxy-alpine:latest.svg)](https://imagelayers.io/?images=rdsubhas/tor-privoxy-alpine:latest 'Get your own badge on imagelayers.io')

The smallest (**15 MB**) docker image with [Tor](https://www.torproject.org/) through [Privoxy](http://www.privoxy.org/), a *privacy enhancing proxy*, based on [Alpine Linux](https://registry.hub.docker.com/_/alpine).

```
docker run -d -p 8118:8118 -p 9050:9050 rdsubhas/tor-privoxy-alpine
curl --proxy localhost:8118 https://www.google.com
```

Read the accompanying [blog post](https://medium.com/@rdsubhas/docker-image-with-tor-privoxy-and-a-process-manager-under-15-mb-c9e344111b61) for more details.

## Configuration

It is possible to define configuration options for the Tor daemon using env variables. Just define a variable prefixed with `TOR_`, as shown in this example:

```
docker run -d -p 8118:8118 -p 9050:9050 -e TOR_ExitNodes="{de}" rdsubhas/tor-privoxy-alpine
```

The `torrc` will now contain:

```
ExitNodes {de}
```
## Customize

To customize the configuration, add volume mount like: `-v $PWD/torrc:/etc/tor/torrc:ro` (see default [sample `torrc` file](https://gitweb.torproject.org/tor.git/plain/src/config/torrc.sample.in)). You may also want to mount what you've set a Tor `DataDirectory` (see [Tor Manual](https://www.torproject.org/docs/tor-manual.html.en#_files) for details of each sub-directory) to avoid losing caches and identities when you restart your container.

## Known Issues

* When running in interactive mode, pressing Ctrl+C doesn't cleanly exit. For now, run it in detached mode (`-d`). Calling `docker stop` cleanly exits though.
* We're using `testing` versions of tor and runit in Alpine. Got to keep an eye on future builds, until those packages reach `main` in Alpine.

## Browser check

Since not all browsers are equal and some require extra steps to be anonymous, you may run some checks for your browser once you're set up:

 * https://anonymous-proxy-servers.net/en/help/security_test.html
 * http://fingerprint.pet-portal.eu/
 * https://panopticlick.eff.org/
 * http://ip.cc/anonymity-test.php

## Other interesting projects

* [s6 supervision suite](http://skarnet.org/software/s6/index.html), similar to runit and daemontools
* [s6-overlay](https://github.com/just-containers/s6-overlay), base container with s6 and alpine
* [docker-slim](https://github.com/cloudimmunity/docker-slim), a tool to automatically analyze and trim existing fat containers
