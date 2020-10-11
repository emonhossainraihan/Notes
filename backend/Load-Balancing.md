# [Load Balancing Node.js Application Servers with Nginx](https://codeforgeek.com/load-balancing-nodejs-servers-nginx/)

## Nginx load balancing methods

You can use the following load balancing methods:

- Round-robin (Default)
- hash
- IP hash
- Least connections
- Least time

### Round-robin

It’s the default method. In this, Nginx runs through the list of upstreams servers in sequence, assigning the next connection request to each one in turn.
Hash

In this method, Nginx calculates a hash that is based on a combination of text and Nginx variables and assigns it to one of the servers. The incoming request matching the hash will go to that specific server only.

Example:

```
upstream app_servers {
    hash $scheme$request_uri;
    server 127.0.0.1:3000;
    server 127.0.0.1:3001;
    server 127.0.0.1:3002;
    server 127.0.0.1:3002;
}
```

### IP Hash

This method is the only available for the HTTP server. In this method, the hash is calculated based on the IP address of a client. This method makes sure the multiple requests coming from the same client go to the same server. This method is required for socket and session-based applications.

Example:

```
upstream app_servers {
    ip_hash;
    server 127.0.0.1:3000;
    server 127.0.0.1:3001;
    server 127.0.0.1:3002;
    server 127.0.0.1:3002;
}
```

### Least connections

In this method, Nginx sends the incoming request to the server with the fewest connection thus maintaining the load across servers.

```
upstream app_servers {
    least_conn;
    server 127.0.0.1:3000;
    server 127.0.0.1:3001;
    server 127.0.0.1:3002;
    server 127.0.0.1:3002;
}
```

### Least time

In this method, Nginx calculates and assigns values to each server by using the current number of active connections and weighted average response time for past requests and sends the incoming request to the server with the lowest value.

It’s a premium feature and only available in Nginx plus.

## Which load balancing method to choose?

It totally depends on the traffic pattern and nature of your application. For a site like Codeforgeek, the least connection method suits the best i.e server the lowest number of active connection should get the request to divide the load evenly and improving performance at the same time.

You can select based on your traffic pattern. For socket-based apps, IP hash suits the best.
