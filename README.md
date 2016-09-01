Steps should be performed from the root of the nats-cnc-exploration project.

# Cleanup
```
docker rm broker.example.com controller01.example.com client01.example.com controller02.example.com
```

# Start servers
```
docker run -v `pwd`:/nats --name broker.example.com nats -c /nats/server1.conf
docker run --link broker.example.com -v `pwd`:/nats --name controller01.example.com nats -c /nats/server2.conf
```

# Create a Docker image called nats_client with cnats installed
```
docker build nats_client -t nats_client
```

# Start agent
```
docker run -it --link broker.example.com --link controller01.example.com -v `pwd`:/nats --name client01.example.com nats_client /cnats/examples/nats-subscriber -s nats://client:bar@broker.example.com:4222 -tls -tlscacert /nats/ssl/certs/ca.pem -tlscert /nats/ssl/certs/client01.example.com.pem -tlskey /nats/ssl/private_keys/client01.example.com.pem
```

# Create a Docker image called nots_top with nats_top installed
```
docker build nats_top -t nats_top
```

# Start monitor
```
docker run -it --link broker.example.com --link controller01.example.com --link client01.example.com nats_top /gocode/bin/nats-top -s broker.example.com -ms 8222 -k -lookup
```

# Start controller
```
docker run -it --link broker.example.com --link controller01.example.com -v `pwd`:/nats --name controller02.example.com nats_client /cnats/examples/nats-publisher -s nats://controller:foo@broker.example.com:4222 -tls -tlscacert /nats/ssl/certs/ca.pem -tlscert /nats/ssl/certs/controller02.example.com.pem -tlskey /nats/ssl/private_keys/controller02.example.com.pem
```
