VERSION 0.6

INSTALL_DEPENDENCIES:
    COMMAND
    COPY "go.mod" "go.mod"
    COPY "go.sum" "go.sum"
    RUN go mod download


COPY_SOURCECODE:
    COMMAND
    COPY "./ci" "./ci"
    COPY "./cmd" "./cmd"
    COPY "./api" "./api"


SAVE_OUTPUT:
    COMMAND
    SAVE ARTIFACT "starling-bank-technical-challenge" AS LOCAL "starling-bank-technical-challenge"
    SAVE ARTIFACT "go.sum" AS LOCAL "go.sum"


golang-base:
    FROM golang:1.19
    WORKDIR /tmp/starling-bank-technical-challenge
    ENV GOPROXY=direct
    ENV CGO_ENABLED=0
    ENV GOOS=linux
    ENV GOARCH=amd64


check-formatting:
    FROM +golang-base
    DO +INSTALL_DEPENDENCIES
    DO +COPY_SOURCECODE
    RUN ./ci/check-formatting.sh


fix-formatting:
    FROM +golang-base
    DO +INSTALL_DEPENDENCIES
    DO +COPY_SOURCECODE
    RUN ./ci/fix-formatting.sh
    SAVE ARTIFACT "./src" AS LOCAL "./src"


linting:
    FROM +golang-base
    RUN go install github.com/golangci/golangci-lint/cmd/golangci-lint@v1.50.0
    DO +INSTALL_DEPENDENCIES
    DO +COPY_SOURCECODE
    RUN ./ci/linting.sh


check-module-tidying:
    FROM +golang-base
    DO +INSTALL_DEPENDENCIES
    DO +COPY_SOURCECODE
    RUN ./ci/check-module-tidying.sh


fix-module-tidying:
    FROM +golang-base
    DO +INSTALL_DEPENDENCIES
    DO +COPY_SOURCECODE
    RUN ./ci/fix-module-tidying.sh
    SAVE ARTIFACT "go.mod" AS LOCAL "go.mod"


compiling-linux-amd64:
    FROM +golang-base
    DO +INSTALL_DEPENDENCIES
    DO +COPY_SOURCECODE
    RUN ./ci/compiling.sh
    DO +SAVE_OUTPUT


compiling-darwin-amd64:
    FROM +golang-base
    ENV GOOS=darwin
    DO +INSTALL_DEPENDENCIES
    DO +COPY_SOURCECODE
    RUN ./ci/compiling.sh
    DO +SAVE_OUTPUT
