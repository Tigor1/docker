FROM ubuntu:18.04

RUN apt-get update -y && apt-get upgrade -y && apt-get install -y ca-certificates \
														openssh-server \
														wget \
														postfix \
														tzdata && \
														apt-get clean -y

ENV TERM xterm

RUN wget https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh && \
																 chmod 777 script.deb.sh && \
																 ./script.deb.sh && apt-get install -y gitlab-ce && apt-get clean -y


EXPOSE 443 80 22

ARG EXTERNAL_URL="192.168.99.106"

RUN	mkdir -p /etc/gitlab/ssl && \
	chmod 755 /etc/gitlab/ssl && \
	openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/gitlab/ssl/${EXTERNAL_URL}.key -out /etc/gitlab/ssl/${EXTERNAL_URL}.crt -subj "/C=RU/ST=Moscow/L=Moscow/O=21-school/OU=docker-1/CN=${EXTERNAL_URL}" && \
	 sed -i "s/\# letsencrypt\['enable'\] = nil/letsencrypt\['enable'\] = false/" /etc/gitlab/gitlab.rb && \
	 sed -i "s/external_url 'http:\/\/gitlab.example.com'/external_url 'https:\/\/${EXTERNAL_URL}:443'/" /etc/gitlab/gitlab.rb

EXPOSE 22 443

RUN mkdir -p /run/sshd

ENTRYPOINT (/opt/gitlab/embedded/bin/runsvdir-start &) && \
							 /usr/sbin/sshd && \
							 gitlab-ctl reconfigure && \
							 tail -f /dev/null

# docker build -t ex03 .
# docker run -it --rm -p 8022:22 -p 8443:443 --privileged ex03
# git config --global http.sslVerify false
