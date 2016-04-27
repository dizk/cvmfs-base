# cvmfs-base

This image runs cvmfs in a container. 

This is very much WIP, use with caution.

```
mkdir -p $HOME/test
docker run -it \
    --privileged \                              # Must run as privileged
    -v $HOME/test:/cvmfs:rshared \              # Mount point
    -e CVMFS_REPOSITORIES=cernvm-prod.cern.ch \ # Repos to mount
    -e CVMFS_HTTP_PROXY="DIRECT" \              # Proxy settings
     cbatchx/cvmfs-base
```

## Requirements
* Docker 1.10+
* You have at least one mountpoint on the host that is shared.

Example (executed on the host):
```
$ df /home/diz/test                                             # Example folder
Filesystem     1K-blocks      Used Available Use% Mounted on
/dev/sdb2      154956440 135298064  11763916  93% /             # Mounted on /


$ findmnt -o TARGET,PROPAGATION /
TARGET PROPAGATION
/      shared                                                   # Mountpoint is shared
```

# Troubleshooting
First of all see: [https://github.com/docker/docker/issues/20345](https://github.com/docker/docker/issues/20345)

On ubuntu 15.04:

Remove `MountFlags=slave` from `/lib/systemd/system/docker.service`
Then run
```sh
sudo systemctl daemon-reload
sudo systemctl restart docker
```