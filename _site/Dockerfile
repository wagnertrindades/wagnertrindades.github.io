
FROM ruby

VOLUME /src

WORKDIR /src
ENTRYPOINT ["jekyll"]

RUN apt-get update
RUN apt-get install -y node python-pygments

RUN gem install jekyll rdiscount kramdown bundler

COPY Gemfile ./Gemfile
COPY Gemfile.lock ./Gemfile.lock

RUN bundle install
