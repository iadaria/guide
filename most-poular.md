### Error: Address already in use

### Error: listen EADDRINUSE

$ lsof -ti :$PORT
$ kill $(lsof -ti :$PORT)
$ sudo kill -9 $(lsof -ti :$PORT)
$ fuser -k $PORT/tcp
