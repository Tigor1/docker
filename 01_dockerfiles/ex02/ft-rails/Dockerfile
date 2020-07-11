FROM ruby
RUN apt-get update -y && apt-get install -y build-essential libpq-dev nodejs sqlite3
RUN gem install bundler

ONBUILD COPY app /opt/
ONBUILD WORKDIR /opt/app

ONBUILD RUN bundle install
ONBUILD RUN rake db:migrate
ONBUILD RUN rake db:seed

# docker build -t ft-rails:on-build .
