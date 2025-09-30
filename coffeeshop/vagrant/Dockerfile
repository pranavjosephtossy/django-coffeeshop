FROM ubuntu:22.04
ENV DEBIAN_FRONTEND=noninteractive

# OS deps
RUN apt-get update && apt-get install -y --no-install-recommends \
    sudo tzdata ca-certificates locales \
    openssh-server \
    python3 python3-dev python3-pip python3-venv python3-setuptools python3-wheel \
    build-essential \
    postgresql postgresql-contrib libpq-dev \
    libsqlite3-dev \
    apache2 libapache2-mod-wsgi-py3 \
    curl git vim nano ruby-full rubygems-integration \
  && rm -rf /var/lib/apt/lists/*

# Python libs needed at runtime
RUN pip3 install --no-cache-dir django psycopg2-binary python-dotenv

# (Optional) Mailcatcher used by lab
RUN gem install mailcatcher

# Vagrant SSH user setup
RUN mkdir -p /var/run/sshd && \
    useradd -m -s /bin/bash vagrant && echo "vagrant:vagrant" | chpasswd && \
    usermod -aG sudo vagrant && \
    echo "vagrant ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/zz-vagrant && \
    chmod 440 /etc/sudoers.d/zz-vagrant && \
    sed -ri 's/^#?PasswordAuthentication.*/PasswordAuthentication yes/' /etc/ssh/sshd_config && \
    sed -ri 's@session\\s*required\\s*pam_loginuid.so@session optional pam_loginuid.so@g' /etc/pam.d/sshd && \
    printf "UseDNS no\nClientAliveInterval 180\nClientAliveCountMax 3\n" >> /etc/ssh/sshd_config

EXPOSE 22
CMD ["/usr/sbin/sshd","-D"]
