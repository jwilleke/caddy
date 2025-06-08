# Caddy Reverse Procy

## Caddy Network

Create the caddy Network and that's where Caddy's going to sit on and it's also the network we're going to put portainer on so we can reference it as portainer and then colon 9443 we don't have to worry about it actual internal IP

``` bash
docker network create caddy
1e2010412256d8485e32b868c0be863a102e949801d64205352196c37e562693

docker network ls
NETWORK ID     NAME                DRIVER    SCOPE
afd9b153f3e3   bridge              bridge    local
1e2010412256   caddy               bridge    local
25368a0f9fef   host                host      local
d40f68da811d   none                null      local
ea16d1a540c6   teslamate_default   bridge    local

```

From /mnt/apps/docker/caady

docker-compose up -d
