FROM ruby:3.1-slim

RUN apt update
RUN apt install -y git build-essential
RUN gem install jekyll bundler

WORKDIR /srv/jekyll

COPY . .

RUN bundle

CMD [ "jekyll", "--serve" ]
