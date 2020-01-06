FROM ruby:2.6

RUN gem install --no-document -v '0.9.20' yard

WORKDIR /app

ONBUILD COPY ruby/ /app/
ONBUILD RUN \
  for dir in $(find /app/ -name '*.gemspec' | xargs dirname); do \
    cd $dir; yard doc --output-dir /docs/$(basename $dir); \
  done
