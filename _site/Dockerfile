# sudo docker build -t wagnertrindade/wagnertrindades-github-io:latest .
# sudo docker run -it -v $(pwd):/src -p 4000:4000 wagnertrindade/wagnertrindades-github-io:latest serve -w --host 0.0.0.0 --port 4000
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
