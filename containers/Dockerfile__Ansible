FROM amazonlinux:2

# ARG USER

RUN yum -y update
RUN yum -y install sudo

RUN amazon-linux-extras enable ansible2
RUN yum install -y ansible

# RUN yum -y update
# RUN yum -y install sudo
# RUN yum -y install openssh-server

# RUN adduser $USER && mkdir -p /home/$USER/.ssh && chown $USER:$USER /home/$USER/.ssh && chmod 0700 /home/$USER/.ssh
# ADD ./authorized_keys.pub /home/$USER/.ssh/authorized_keys
# RUN chown $USER:$USER /home/$USER/.ssh/authorized_keys && chmod 0600 /home/$USER/.ssh/authorized_keys
# RUN echo "$USER ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# CMD /etc/init.d/sshd start && tail -f /dev/null
