# mp-bin
Example on how to build a binary for multiple platforms using Go

### Build a binary compatible with the caller's local machine:
```
earthly github.com/idodod/mp-bin+build-a-binary --VERSION=v1.2.3
```
#### Run the binary:
```
./binaries/mp-bin-v1.2.3
```

### Build a binary compatible with several platforms:
```
earthly github.com/idodod/mp-bin+release --VERSION=v1.2.3
```
