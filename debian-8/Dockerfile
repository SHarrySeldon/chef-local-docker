FROM debian:8
MAINTAINER Xabier de Zuazo "xabier@zuazo.org"

ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/opt/chef/bin:/opt/chef/embedded/bin \
    APT_ARGS="-y --no-install-recommends --no-upgrade -o Dpkg::Options::=--force-confnew" \
    CHEF_REPO_PATH=/tmp/chef
ENV COOKBOOK_PATH=$CHEF_REPO_PATH/cookbooks

# Install chef-client
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive \
    apt-get install $APT_ARGS \
      ca-certificates \
      curl && \
    curl https://packages.chef.io/files/stable/chefdk/1.5.0/debian/8/chefdk_1.5.0-1_amd64.deb && \
    dpkg -i chefdk_1.5.0-1_amd64.deb 

# Configure locales
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
    DEBIAN_FRONTEND=noninteractive \
    apt-get install $APT_ARGS \
      locales && \
    update-locale LANG=en_US.UTF-8
ENV LANG=en_US.UTF-8 LANGUAGE=en_US:en LC_ALL=en_US.UTF-8

# Configure Chef Client
RUN mkdir -p $COOKBOOK_PATH && \
    mkdir -p /etc/chef ~/.chef && \
    echo "cookbook_path %w($COOKBOOK_PATH)" > /etc/chef/client.rb && \
    echo "local_mode true" >> /etc/chef/client.rb && \
    echo "chef_zero.enabled true" >> /etc/chef/client.rb && \
    ln /etc/chef/client.rb ~/.chef/config.rb

# Some optional but recommended packages (+33 MB)
RUN DEBIAN_FRONTEND=noninteractive \
    apt-get install $APT_ARGS \
       git

# Clean and remove not required packages
RUN DEBIAN_FRONTEND=noninteractive \
    apt-get autoremove -y && \
    rm -rf /var/cache/apt/archives/*

CMD ["bash"]
