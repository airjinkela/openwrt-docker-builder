## openwrt docker编译环境
    用于隔离宿主机和编译环境
    比如openwrt-21.02需要旧版本的gcc编译器, 但是宿主机不能降级或安装旧版本的gcc时
    就可以创建一个docker编译环境, 也不怕弄炸宿主机环境

# Prepare
```BASH
ssh-keygen -t ed25519 -C "example@example.com" -f authorized_keys
```

# Build
```BASH
docker build \
    --build-arg AUTHORIZED_KEY="$(cat ../authorized_keys.pub)" \
    --build-arg USERNAME=<USER_NAME> \
    --build-arg UID=<UID> #optional \
    --build-arg GID=<GID> #optional \
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
ssh -o IdentityFile=authorized_keys -p <PORT> <USER_NAME>@<HOST>
```