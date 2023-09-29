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
Using the command `docker-compose up` works like a charm and the application is available through http://localhost:3000.

But once we build the container using `docker build . -t my-image` and run using `docker run --rm my-image`, the application does
not run, but shows the output of the command `rails` instead. This happen because we are trying to start up rails but the project
files are not inside the container. To solve this we can add the project files to the image:

Dockerfile
```
FROM ruby:3.1.2

WORKDIR /api

COPY Gemfile ./
COPY Gemfile.lock ./

RUN bundle install

# Here we add the project files to the container image
ADD . /api 

CMD ["rails", "s", "-b", "0.0.0.0"]
```

Running the command `docker-compose up api` after `docker-compose build --no-cache api` we can confirm the image is
working normally with `docker-compose`.

We can build the image using the command `docker build . -t my-image` again, but if we run `docker run --rm my-image`,
the application will start correctly this time. But once we try to access the application through the browser, we do not have a response from
the application itself. The browser is returning 'localhost refused to connect'.

The root of the error is because the port of the container is not exposed, so we have to pass the ports argument using `-p 3000:3000`
so the host can link the container port and the host port. Running `docker run --rm -p 3000:3000 my-image` and the application is
accessible from the browser!
