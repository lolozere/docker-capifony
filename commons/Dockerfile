FROM ubuntu:trusty

# user account used to run apps
ENV WORKER_UID 1000
ENV WORKER_NAME appuser

ENV LANG fr_FR.UTF-8
ENV LANGUAGE fr_FR
ENV TZ "Europe/Paris"
ENV DEBIAN_FRONTEND noninteractive

# Useful packages
RUN apt-get update
RUN locale-gen fr_FR.UTF-8 && export LANG=fr_FR.UTF-8 && DEBIAN_FRONTEND=noninteractive && export DEBIAN_FRONTEND
RUN apt-get install -y nano wget unzip build-essential

# Enable terminal for nano
ENV TERM xterm

# Create user now to run capifony
RUN useradd --create-home --user-group --shell /bin/bash appuser && \
	echo appuser:password | chpasswd

# Dependencies for ruby
RUN apt-get install -y libssl-dev libreadline-dev zlib1g-dev git
RUN apt-get install -y libgdbm-dev libncurses5-dev automake libtool bison libffi-dev
RUN apt-get install -y curl

# Download and install RVM with a default version
RUN gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
RUN /bin/bash -l -c "curl -L get.rvm.io | bash -s stable"
RUN usermod -a -G rvm root &&  usermod -a -G rvm appuser
RUN /bin/bash -l -c "source /etc/profile.d/rvm.sh"

# Install RVM required
RUN /bin/bash -l -c "rvm install 2.3.1"
RUN /bin/bash -l -c "rvm use 2.3.1 --default"

# Workdir where app source deployed will be mounted
WORKDIR /home/appuser/app

# script for entry point
ADD capifony.sh /home/appuser/capifony.sh
RUN chmod u+x /home/appuser/capifony.sh && chown appuser:appuser /home/appuser/capifony.sh

# Use ssh key and agent from host
RUN mkdir /home/appuser/.ssh
ENV SSH_AUTH_SOCK /home/appuser/ssh-agent
VOLUME ["/home/appuser/.ssh", "/home/appuser/app"]

USER appuser
ENTRYPOINT ["/home/appuser/capifony.sh"]
