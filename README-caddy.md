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

## Common commands needed

``` bash
docker compose logs caddy

docker compose down

docker-compose up -d

docker exec -it caddy /bin/sh


```

## Configure the Token:

Token Name: Enter a descriptive name, e.g., caddy-dns-nerdsbythehour.

### Permissions:

- Add: Zone > Zone > Read (to read zone details).
- Add: Zone > DNS > Edit (to create DNS records for the dns-01 challenge).

### Zone Resources

- Select Include > Specific zone > nerdsbythehour.com. This ensures the token only applies to your domain.
- Optional Restrictions (recommended for security):
- Client IP Address Filtering: Restrict to your server’s IP if static.
- TTL: Set an expiration date (e.g., 1 year) for security.
- Click Continue to summary
- Copy key as you will never see it again!

test the key

``` bash
curl "https://api.cloudflare.com/client/v4/user/tokens/verify" -H "Authorization: Bearer YourVerySecretToken"
```

## Troubleshooting

```bash
sudo netstat -tulnp
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      1366498/docker-prox
tcp        0      0 0.0.0.0:2019            0.0.0.0:*               LISTEN      1366457/docker-prox
tcp        0      0 0.0.0.0:443             0.0.0.0:*               LISTEN      1366479/docker-prox


wget --spider http://localhost

docker exec -it caddy ls /usr/share/caddy
index.html

docker exec -it caddy ls -l /usr/share/caddy/index.html
-rwxr-xr-x    1 3000     3000           225 Jun  8 17:50 /usr/share/caddy/index.html

curl -v http://localhost/index.html
*   Trying 127.0.0.1:80...
* Connected to localhost (127.0.0.1) port 80 (#0)
> GET /index.html HTTP/1.1
> Host: localhost
> User-Agent: curl/7.88.1
> Accept: */*
>
< HTTP/1.1 404 Not Found
< Content-Type: text/plain; charset=utf-8
< X-Content-Type-Options: nosniff
< Date: Mon, 09 Jun 2025 09:41:08 GMT
< Content-Length: 19
<
404 page not found
* Connection #0 to host localhost left intact

 wget --spider http://localhost/
Spider mode enabled. Check if remote file exists.
--2025-06-09 12:51:17--  http://localhost/
Resolving localhost (localhost)... ::1, 127.0.0.1
Connecting to localhost (localhost)|::1|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 225 [text/html]
Remote file exists and could contain further links,
but recursion is disabled -- not retrieving.

# How can this be
curl -I http://localhost/index.html
HTTP/1.1 404 Not Found
Content-Type: text/plain; charset=utf-8
X-Content-Type-Options: nosniff
Date: Mon, 09 Jun 2025 16:56:53 GMT
Content-Length: 19


wget --spider http://localhost/index.html
Spider mode enabled. Check if remote file exists.
--2025-06-09 12:55:24--  http://localhost/index.html
Resolving localhost (localhost)... ::1, 127.0.0.1
Connecting to localhost (localhost)|::1|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 225 [text/html]
Remote file exists and could contain further links,
but recursion is disabled -- not retrieving.

docker inspect caddy | grep -A 5 Mounts
        "Mounts": [
            {
                "Type": "bind",
                "Source": "/mnt/apps/docker/caddy/Caddyfile",
                "Destination": "/etc/caddy/Caddyfile",
                "Mode": "rw",


127.0.0.1
wget --spider http://127.0.0.1/index.html

curl -I http://127.0.0.1/index.html
```
