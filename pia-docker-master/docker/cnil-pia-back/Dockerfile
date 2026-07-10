FROM ruby:3.4.9

ARG PIA_VERSION=4.1.0
ARG PIA_BRANCH=sso

ENV DEBIAN_FRONTEND=noninteractive
ENV RAILS_ENV=production
ENV RAILS_SERVE_STATIC_FILES=true

RUN apt-get update \
 && apt-get install --no-install-recommends -y \
    git \
    wget \
    unzip \
    postgresql-client \
    shared-mime-info \
 && apt-get clean

#TAG RUN wget https://github.com/LINCnil/pia-back/archive/refs/tags/v$PIA_VERSION.zip \
#BRANCH
RUN wget https://github.com/LINCnil/pia-back/archive/$PIA_BRANCH.zip \
 && unzip $PIA_BRANCH.zip \
 && rm $PIA_BRANCH.zip \
 && mv pia-back-$PIA_BRANCH /var/www

# Local git clone in subdirectory pia-back
#COPY pia-back /var/www

COPY database.yml /var/www/config/database.yml
COPY application.yml /var/www/config/application.yml

WORKDIR /var/www/

#RUN gem install bundler
RUN bundle config set without 'test'
RUN bundle config set without 'development'
RUN bundle install 

COPY entrypoint /entrypoint
RUN chmod +x /entrypoint
ENTRYPOINT ["/entrypoint"]

CMD ["bin/rails", "server"]
