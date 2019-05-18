#Docker can be useful to test an application that's still being developed without
#polluting your libraries. You will have to design a Dockerfile that gets the
#development version of Gitlab installs it with all the dependencies and the
#necessary configurations, and launches the application, all as it builds. The
#container will be deemed valid if you can acess the web client, create users and
#interact via GIT with this container (HTTPS and SSH). Obviously, you are not
#allowed to use the official container from Gitlab, it would be a shame...

FROM ubuntu:16.04

# Updating default system
RUN apt-get update -y && apt-get upgrade -y
# Installing needed resources
RUN apt-get install -y wget openssh-server ca-certificates vim
# Installing postfix -- Email notifications
RUN apt-get install -y postfix
# Getting debian script to setup gitlab && installing it
RUN wget https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh && chmod +x script.deb.sh && ./script.deb.sh && apt-get install -y gitlab-ce
# Line to patch infinite loop when installing gitlab
RUN apt-get install -y tzdata && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /script.deb.sh
# Expose web and ssh
EXPOSE 443 80 22
# Creating certificates
RUN mkdir -p /etc/gitlab/ssl/ && cd /etc/gitlab/ssl/ && openssl genrsa -out gitlab.domain.com.key 2048 && openssl req -new -x509 -key gitlab.domain.com.key -out gitlab.domain.com.crt -days 3650 -subj /CN=gitlab.domain.com
# Let's encrypt disabled -- Use our certificates
RUN echo "letsencrypt['enable'] = false" >> /etc/gitlab/gitlab.rb
# Setting gitlab ssh port
RUN echo "gitlab_rails['gitlab_shell_ssh_port'] = 2222" >> /etc/gitlab/gitlab.rb
# Changing the external_url to an https one
RUN sed -i "s/^external_url.*/external_url \'https:\/\/gitlab.domain.com\'/" /etc/gitlab/gitlab.rb
# Commands to reconfigure and start the gitlab
RUN (/opt/gitlab/embedded/bin/runsvdir-start &) && gitlab-ctl reconfigure
# Commands called when docker, tailed to keep it running
#  Starts both ssh and gitlab
ENTRYPOINT service ssh start && (/opt/gitlab/embedded/bin/runsvdir-start &) && gitlab-ctl start && tail -f /dev/null

# To build
# docker build -t ex03 .
# To run
# docker run --rm -it -p 80:80 -p 443:443 -p 2222:22 ex03
# Add more power to the VM:
# docker-machine stop Char && VBoxManage modifyvm Char --cpus 2 && VBoxManage modifyvm Char --memory 4096 && docker-machine start Char && eval $(docker-machine env Char)
# Before pushing on Gitlab, add command GIT_SSL_NO_VERIFY=true
