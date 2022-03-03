FROM debian:unstable

LABEL maintainer="Michael Buluma"

ARG DEBIAN_FRONTEND=noninteractive

ENV pip_packages "ansible cryptography"
# ENV pip_packages "cryptography"

RUN echo 'deb http://deb.debian.org/debian experimental main' > /etc/apt/sources.list.d/experimental.list

# Install dependencies.
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
       sudo systemd systemd-sysv \
       build-essential wget libffi-dev libssl-dev \
       python3-pip python3-dev python3-setuptools python3-wheel python3-apt ansible \
    && rm -rf /var/lib/apt/lists/* \
    && rm -Rf /usr/share/doc && rm -Rf /usr/share/man \
    && apt-get clean

# Uninstall current ansible
RUN pip uninstall ansible ansible-base

# Upgrade pip to latest version.
RUN pip3 install --upgrade pip

# Lets check Ansibe version present
RUN ansible --version

# Install Ansible via pip.
RUN pip3 install $pip_packages

# COPY initctl_faker .
# RUN chmod +x initctl_faker && rm -fr /sbin/initctl && ln -s /initctl_faker /sbin/initctl

# Install Ansible inventory file.
RUN mkdir -p /etc/ansible
RUN echo "[local]\nlocalhost ansible_connection=local" > /etc/ansible/hosts

# Make sure systemd doesn't start agettys on tty[1-6].
RUN rm -f /lib/systemd/system/multi-user.target.wants/getty.target

VOLUME ["/sys/fs/cgroup"]
CMD ["/lib/systemd/systemd"]
