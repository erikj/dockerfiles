FROM centos:centos6

# install java / openjdk

# https://github.com/docker-library/java/blob/2408508f3b000bf997febbdb90a861185a64a260/openjdk-8-jdk/Dockerfile

RUN yum -y install java-1.8.0-openjdk-headless

# install jruby

RUN yum -y install tar

# https://github.com/cpuguy83/docker-jruby/blob/e7af3d9a85d166f3dd4b600a629908bd05639fb5/1.7/Dockerfile

ENV JRUBY_VERSION 1.7.18
RUN mkdir /opt/jruby \
  && curl http://jruby.org.s3.amazonaws.com/downloads/${JRUBY_VERSION}/jruby-bin-${JRUBY_VERSION}.tar.gz \
  | tar -zxC /opt/jruby --strip-components=1 \
  && update-alternatives --install /usr/local/bin/ruby ruby /opt/jruby/bin/jruby 1
ENV PATH /opt/jruby/bin:$PATH

RUN echo 'gem: --no-rdoc --no-ri' >> /.gemrc

ENV GEM_HOME /usr/local/bundle
ENV PATH $GEM_HOME/bin:$PATH
RUN gem install bundler

# RUN gem install bundler \
#   && bundle config --global path "$GEM_HOME" \
#   && bundle config --global bin "$GEM_HOME/bin"

# # don't create ".bundle" in all our apps
# ENV BUNDLE_APP_CONFIG $GEM_HOME

CMD [ "irb" ]
