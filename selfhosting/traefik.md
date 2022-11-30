## Let's Encrypt staging environment
While testing configurations, it's recommended to use the Let's Encrypt staging enviroment, to avoid being rate limited by the production environment.

Set the `caserver` property of a traefik certificate resolver to use the staging environment:
```
--certificatesresolvers.letsencrypt.acme.caserver=https://acme-staging-v02.api.letsencrypt.org/directory
```
## Wildcard certificates with acme
Wildcard certificates are only available using the `DNS-01` challange. This means, a [provider](https://doc.traefik.io/traefik/https/acme/#providers) (API) is required, to automatically set DNS records. Providers I have used successfully are [duckdns](https://duckdns.org) and [Hetzner](https://dns.hetzner.com/) .

### Default `tls` configuration
A default tls configuration can be set on an entrypoint, which is then applied to every router using that entry point:
```
# use the certificate resolver called letsencrypt
--entrypoints.https.http.tls.certResolver=letsencrypt

# specify root domain and wildcard domain as SAN
--entrypoints.https.http.tls.domains[0].main=example.com
--entrypoints.https.http.tls.domains[0].sans=*.example.com
```
Note, that if you specify the `tls` property on a router, the default configuration is overwritten and has no effect whatsoever.
### Defining a certificate resolver
```
# email for expiration notices, etc.
--certificatesResolvers.letsencrypt.acme.email=mail@example.com

# storage of the certificates, make sure these are mounted in a docker volume
--certificatesResolvers.letsencrypt.acme.storage=/etc/traefik/acme.json

# enable dns challange
--certificatesResolvers.letsencrypt.acme.dnschallenge=true

# set provider
--certificatesResolvers.letsencrypt.acme.dnschallenge.provider={{ PROVIDER }}

# set a delay in seconds after before verifying the challange
--certificatesResolvers.letsencrypt.acme.dnschallenge.delaybeforecheck=10

# use these nameservers for verification
--certificatesResolvers.letsencrypt.acme.dnschallenge.resolvers=1.1.1.1:53,8.8.8.8:53
```
Note, also credentials for the provider need to be set as an environment variables:

```
environment:
- DUCKDNS_TOKEN={{ TOKEN }}
# or
- HETZNER_API_KEY={{ API_KEY }}
```
### Adding a service
Just add service, using the base domain in the Host() rule:
```
labels:
	traefik.enable: "true"
	traefik.http.routers.service.rule: "Host(`service.example.com`)"
```

