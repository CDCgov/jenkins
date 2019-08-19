FROM openshift/jenkins-slave-base-centos7

MAINTAINER Tim Taylor <ttaylor@mitre.org>

ENV RUBY_VERSION=2.5 \
    YARN_VERSION=1.13.0 \
    PATH=/opt/rh/rh-nodejs10/root/usr/bin:/opt/rh/rh-ruby25/root/usr/local/bin:/opt/rh/rh-ruby25/root/usr/bin:${HOME}/bin:$PATH \
    BASH_ENV=/usr/local/bin/scl_enable \
    ENV=/usr/local/bin/scl_enable \
    LD_LIBRARY_PATH=/opt/rh/rh-nodejs10/root/usr/lib64:/opt/rh/rh-ruby25/root/usr/local/lib64:/opt/rh/rh-ruby25/root/usr/lib64 \
    X_SCLS='rh-nodejs10 rh-ruby25 ' \
    HEADLESS=true \
    PROMPT_COMMAND=". /usr/local/bin/scl_enable"

COPY contrib/bin/scl_enable /usr/local/bin/scl_enable

# Install Ruby
RUN INSTALL_PKGS="rh-ruby25 rh-ruby25-ruby-devel rh-ruby25-rubygem-rake rh-ruby25-rubygem-bundler rh-nodejs10 postgresql-devel qtwebkit-devel xorg-x11-server-Xvfb mesa-dri-drivers make gcc fontconfig bzip2" && \
    yum-config-manager --enable centos-sclo-rh-testing && \
    yum install -y --setopt=tsflags=nodocs epel-release && \
    yum install -y --enablerepo=centosplus --setopt=tsflags=nodocs $INSTALL_PKGS && \
    yum install -y https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm && \
    rpm -V $INSTALL_PKGS && \
    yum clean all -y && \
    systemd-machine-id-setup && \
    alternatives --install /usr/bin/qmake qmake /usr/bin/qmake-qt4 100 && \
    curl -L https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-2.1.1-linux-x86_64.tar.bz2 | \
      tar -jx && \
    ln -s /opt/yarn-v$YARN_VERSION/bin/yarm /usr/bin/yarm && \
    curl -O -L https://chromedriver.storage.googleapis.com/2.35/chromedriver_linux64.zip && \
    unzip ./chromedriver_linux64.zip -d /opt && \
    ln -s /opt/chromedriver /usr/local/bin/chromedriver
 
 # Put in place a config file that will use our custom settings
 COPY contrib/npmrc /opt/rh/rh-nodejs/root/usr/etc/npmrc

 RUN chown -R 1001:0 $HOME && \
     chmod -R g+rw $HOME

USER 1001
