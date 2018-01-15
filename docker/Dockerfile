FROM ubuntu:16.04

RUN "sh" "-c" "echo nameserver 8.8.8.8 > /etc/resolv.conf"
RUN "sh" "-c" "echo nameserver 8.8.4.4 > /etc/resolv.conf"


RUN apt-get update && apt-get install -y python-software-properties software-properties-common libcurl3 libcurl3-gnutls libcurl4-openssl-dev locales mc tzdata

RUN export LANG=en_US.UTF-8
RUN export LANGUAGE=en_US.UTF-8
RUN export LC_ALL=en_US.UTF-8
RUN dpkg-reconfigure locales

# Note: The official Debian and Ubuntu images automatically ``apt-get clean``
# after each ``apt-get``

USER root

RUN apt-get install -y curl apt-transport-https

RUN curl -sL https://deb.nodesource.com/setup_6.x | bash -

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

# Installing some basic and usefull software
RUN apt-get update --fix-missing
RUN apt-get install -y curl git libpq-dev wget p7zip-full imagemagick libmysqlclient-dev libmysqlclient-dev nodejs mc screen yarn


RUN gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3

RUN \curl -L https://get.rvm.io | bash -s stable
RUN /bin/bash -l -c "source /etc/profile.d/rvm.sh"
RUN /bin/bash -l -c "rvm requirements"
RUN /bin/bash -l -c "rvm install 2.5.0"
RUN /bin/bash -l -c "gem install tzinfo-data"

RUN npm install -g @angular/cli

WORKDIR /tmp

COPY Gemfile Gemfile
COPY Gemfile.lock Gemfile.lock
RUN /bin/bash -l -c "gem install bundler"
RUN /bin/bash -l -c "bundle install"

ADD . /todo
WORKDIR /todo

RUN yarn install

CMD /bin/bash -l -c "RAILS_ENV=production rake db:migrate && rm -rf /todo/tmp/* && RAILS_ENV=production  rails s -b 0.0.0.0 -p 80 -e production"
