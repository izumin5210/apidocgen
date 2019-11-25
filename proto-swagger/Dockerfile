FROM buildpack-deps:buster-scm

ENV PROTO_VERSION 3.10.1
ENV GRPC_GATEWAY_VERSION 1.12.1

RUN apt update && apt install unzip

RUN PROTOC_ZIP=protoc-$PROTO_VERSION-linux-x86_64.zip && \
  curl -OL https://github.com/protocolbuffers/protobuf/releases/download/v$PROTO_VERSION/$PROTOC_ZIP && \
  unzip -o $PROTOC_ZIP -d /usr/local bin/protoc && \
  unzip -o $PROTOC_ZIP -d /usr/local 'include/*' && \
  rm -f $PROTOC_ZIP

RUN curl -sfL -o /usr/local/bin/protoc-gen-swagger \
  https://github.com/grpc-ecosystem/grpc-gateway/releases/download/v$GRPC_GATEWAY_VERSION/protoc-gen-swagger-v$GRPC_GATEWAY_VERSION-linux-x86_64 && \
  chmod +x /usr/local/bin/protoc-gen-swagger

RUN curl -sfL https://github.com/grpc-ecosystem/grpc-gateway/archive/v$GRPC_GATEWAY_VERSION.tar.gz | \
  tar xz && \
  mv grpc-gateway-$GRPC_GATEWAY_VERSION /usr/local/include/grpc-gateway

WORKDIR /app

RUN mkdir /docs

ONBUILD COPY proto/ /app/
ONBUILD RUN \
  for dir in $(find . -name '*.proto' -type f | grep -vE '^./vendor/' | xargs dirname | sort | uniq); do \
    mkdir -p /docs/$dir; \
    protoc \
      -I . \
      -I /usr/local/include/grpc-gateway \
      -I /usr/local/include/grpc-gateway/third_party/googleapis \
      --swagger_out=logtostderr=true,allow_merge=true,merge_file_name=index:/docs/$dir \
      $dir/*.proto; \
  done
