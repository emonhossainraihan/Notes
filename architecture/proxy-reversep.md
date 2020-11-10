## Proxy Use Cases 

```
client > proxy > server
```

- Logging
- Anonymity
- Geofencing 
- Caching
- Block sites
- Enable Polyglot 

## Reverse Proxy Use Cases 

```
                                /-> server-1
client > server > reverse-proxy 
                                \-> server-2
```

- Caching 
- Load Balancing 
- Ingress
- Canary Deployment
- Microservices

## Networking

SSL and TLS are both cryptographic protocols that provide authentication and data encryption between servers, machines, and applications operating over a network (e.g. a client connecting to a web server).

>  HTTPS is everywhere thanks mostly to Let’s Encrypt, and that’s a good thing! A secure web is a better web, but what about HTTPS performance? HTTPS has always had a bad rep when it comes to performance, because the TLS handshake is more CPU intensive.

- If Varnish isn’t the quickest solution and the most difficult to setup, why on earth would you opt for it? Quite simply, Varnish is still the best at handling more complex cache invalidation rules.
- Nginx FastCGI does support HTTPS protocol, which is an excellent alternative to Varnish, without having to increase any complexity in the server.

Server Name Indication (SNI) is an extension of the TLS protocol. The client specifies which hostname they want to connect to using the SNI extension in the TLS handshake. This allows a server (for example Apache, Nginx, or a load balancer such as HAProxy) to select the corresponding private key and certificate chain that are required to establish the connection from a list or database while hosting all certificates on a single IP address.

When SNI is used, the hostname of the server is included in the TLS handshake, which enables HTTPS websites to have unique TLS Certificates, even if they are on a shared IP address.
