VERSION --arg-scope-and-set 0.7

ARG GOLANG_VERSION=1.21-alpine
FROM golang:$GOLANG_VERSION
WORKDIR /src

# release produces the binary for several platforms
release:
    FROM alpine
    WORKDIR /artifacts
    ARG VERSION=dev
    BUILD --platform=linux/amd64 --platform=linux/arm64 \ # add other platforms here
    +build-a-binary --VERSION=$VERSION --USE_TARGET_PLATFORM=true

# binary produce a binary
build-a-binary:
    FROM +src
    ARG OUTPUT_DIR="./binaries"
    ARG USE_TARGET_PLATFORM=false
    RUN mkdir -p bin
    ARG VERSION=dev
    LET GOOS=""
    LET GOARCH=""
    LET BIN_NAME=""
    IF [ $USE_TARGET_PLATFORM = "true"  ]
        ARG TARGETOS
        ARG TARGETARCH
        SET GOOS=$TARGETOS
        SET GOARCH=$TARGETARCH
        SET BIN_NAME=mp-bin-$VERSION-$GOOS-$GOARCH
    ELSE
        ARG USEROS
        ARG USERARCH
        SET GOOS=$USEROS
        SET GOARCH=$USERARCH
        SET BIN_NAME=mp-bin-$VERSION
    END
    LET BIN_PATH=bin/$BIN_NAME
    RUN go build -ldflags="-s -w -X 'main.version=${VERSION}'" -o $BIN_PATH ./cmd/...
    SAVE ARTIFACT $BIN_PATH $BIN_NAME AS LOCAL $OUTPUT_DIR/$BIN_NAME

# src is a helper target with the required dependencies and source code to build the binary
src:
    FROM +deps
    COPY --dir cmd .

# deps is a helper target with the downloaded dependencies
deps:
    COPY go.mod go.sum ./
    RUN go mod download
    SAVE ARTIFACT go.mod
    SAVE ARTIFACT go.sum
