FROM ruby:2.6

RUN gem install --no-document -v '0.9.20' yard

WORKDIR /app

ONBUILD COPY ruby/ /app/
ONBUILD RUN \
  for dir in $(find . -name '*.gemspec' | xargs dirname); do \
    yard doc --output-dir /docs/$(basename $dir) $dir; \
  done
