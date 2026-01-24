## Build
```BASH
docker build \
    --build-arg AUTHORIZED_KEY="$(cat ../authorized_keys-example)" \
    --build-arg USERNAME=<USER_NAME> \
    --build-arg UID=<UID> \
    --build-arg GID=<GID> \
    -t ubuntu-2204-opbuild \
    .
```

## Run
```BASH
docker run -d \      
        --name ubuntu-build \
        -p 2222:22 \
        ubuntu-2204-opbuild
```

## Connect
```BASH
ssh <USER_NAME>@localhost -p 2222
```