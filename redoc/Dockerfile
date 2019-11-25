FROM node:12

RUN npm install -g redoc-cli

WORKDIR /app

RUN mkdir /docs

ONBUILD COPY --from=swagger docs/ /app/
ONBUILD RUN \
  for f in $(find /app -name '*.swagger.json' -type f); do \
    echo redoc-cli bundle --output $(echo $f | sed 's/\.swagger\.json$/.html/' | sed 's!^/app/!/docs/!') $f; \
    redoc-cli bundle --output $(echo $f | sed 's/\.swagger\.json$/.html/' | sed 's!^/app/!/docs/!') $f; \
  done
