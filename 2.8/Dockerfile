FROM lolozere/capifony:base

# Install capifony as the deployment's user (appuser)
USER appuser
RUN	/bin/bash -l -c "gem install capifony -v 2.8.6" && \
	/bin/bash -l -c "gem install capistrano_rsync_with_remote_cache"

USER root
RUN ln -s $(find / -name "cap" 2> /dev/null | grep wrappers) /bin/cap

USER appuser