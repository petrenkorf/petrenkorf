Consider the following files:

Dockerfile
```
FROM ruby:3.1.2

WORKDIR /api

COPY Gemfile ./
COPY Gemfile.lock ./

RUN bundle install

CMD ["rails", "s", "-b", "0.0.0.0"]
```
docker-compose.yaml
```
services:
  api:
    build:
      dockerfile: Dockerfile
      context: .
    ports:
      - "3000:3000"
    volumes:
      - .:/api
```


