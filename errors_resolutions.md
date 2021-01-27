### ERROR 

<img src="https://i.imgur.com/4icjMLq.png" />

### RESOLUTION 

Nothing as the mongod service is already running

### ERROR 

<img src="https://i.imgur.com/15Q210N.png" />

Although the issue references a missing data directory this could also be an issue with the brew service 

### RESOLUTION 

Try to first stop the mongo service

```
brew services start mongodb-community@4.4
```

Then start the mongo service

```
brew services stop mongodb-community@4.4
```

To confirm that the service is started view brew services

```
brew services list
```

<img src="https://i.imgur.com/QzKjFiW.png">

To run MongoDB (i.e. the mongod process) manually as a background process, issue the following:

```
mongod --config /usr/local/etc/mongod.conf --fork
```