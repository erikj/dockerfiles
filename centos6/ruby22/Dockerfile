FROM centos:centos6
MAINTAINER Erik Johnson "erik@erikj.me"

# based on https://github.com/docker-library/ruby/blob/b7fefd2fa79882da90feb0718430680c77c5fa8b/2.2/Dockerfile

ENV RUBY_MAJOR 2.2
ENV RUBY_VERSION 2.2.0

ENV BUILD_PKGS tar autoconf automake gcc libffi-devel openssl-devel zlib-devel readline-devel

ENV RM_PKGS autoconf perl libselinux-devel kernel-headers cpp glibc-headers glibc-devel ppl m4 mpfr keyutils-libs-devel keyutils-libs-devel libcom_err-devel libgomp libsepol-devel

RUN yum -y update \
  && yum -y install $BUILD_PKGS \
  && mkdir -p /usr/src/autoconf \
  && curl -SL http://ftp.gnu.org/gnu/autoconf/autoconf-2.69.tar.gz \
    | tar -xzC /usr/src/autoconf --strip-components=1 \
  && cd /usr/src/autoconf \
  && ./configure --prefix=/usr \
  && make \
  && make install \
  && rm -rf /usr/src/autoconf \
  && mkdir -p /usr/src/ruby \
  && cd /usr/src/ruby \
  && curl -SL http://cache.ruby-lang.org/pub/ruby/$RUBY_MAJOR/ruby-$RUBY_VERSION.tar.gz \
    | tar -xzC /usr/src/ruby --strip-components=1 \
  && autoconf \
  && ./configure --disable-install-doc \
  && make -j"$(nproc)" \
  && make install \
  && rm -r /usr/src/ruby \
  && yum -y --setopt=tsflags=noscripts remove $BUILD_PKGS $RM_PKGS \
  && yum -y clean all

# --setopt=tsflags=noscripts resolves Error in PREUN scriptlet in rpm package libffi-devel
# http://www.linux-pages.com/2012/08/error-in-preun-scriptlet-file-was-supposed-to-be-removed-but-is-not/

# install bundler
RUN echo 'gem: --no-rdoc --no-ri' >> "$HOME/.gemrc"
RUN gem install bundler

# install things globally, for great justice
# ENV GEM_HOME /usr/local/bundle
# ENV PATH $GEM_HOME/bin:$PATH
# RUN gem install bundler \
#   && bundle config --global path "$GEM_HOME" \
#   && bundle config --global bin "$GEM_HOME/bin"

# # don't create ".bundle" in all our apps
# ENV BUNDLE_APP_CONFIG $GEM_HOME

CMD ['irb']
