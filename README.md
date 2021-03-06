# Dockerized [rmate](https://github.com/aurora/rmate)

[![Docker Repository on Quay](https://quay.io/repository/jeremejevs/rmate/status
"Docker Repository on Quay")](https://quay.io/repository/jeremejevs/rmate)

Useful for when extra runtimes, like Ruby or Python, are undesired, and even the
bash version doesn't work, like on CoreOS (since its bash has been compiled
without net redirections) or in some non-bash distro (in which case you'll have
to adjust the included rmate script by yourself).

And it's just **8.246 MB**, thanks to
[`gliderlabs/alpine`](https://github.com/gliderlabs/docker-alpine).

### Setup

```bash
wget -O /usr/local/bin/rmate https://raw.githubusercontent.com/jeremejevs/rmate/master/rmate
chmod +x /usr/local/bin/rmate
```

Just in case, `/usr` isn't writable in CoreOS, so use `/opt/bin/rmate` instead,
and since I mention CoreOS specifics, might as well throw in a cloud config,
right?

```yml
coreos:
  units:
    - name: rmate.service
      command: start
      content: |
        [Unit]
        Description=Download and install rmate
        [Service]
        Type=oneshot
        ExecStart=/usr/bin/mkdir -p /opt/bin
        ExecStart=/usr/bin/wget -O /opt/bin/rmate https://raw.githubusercontent.com/jeremejevs/rmate/master/rmate
        ExecStart=/usr/bin/chmod +x /opt/bin/rmate
```

Feel free to replace `/opt/bin/rmate` with `/opt/bin/rsub`, if you prefer that.
I know I do.

### Usage

```bash
rmate foo
rmate bar --name baz
rmate --line 42 qux # won't work, file path should come first
rmate quux --wait # --[no-]wait has no effect
```
