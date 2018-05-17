FROM ubuntu:xenial-20171006

RUN apt-get update

RUN apt-get -y install python-dev build-essential libssl-dev libffi-dev \
                       python-pip python-virtualenv git

CMD mkdir -p /usr/lib/my/

WORKDIR /usr/lib/my/

RUN virtualenv ansible

RUN . ansible/bin/activate && pip install ansible==2.4.0.0

RUN mkdir -p /etc/my/ansible/

ARG ANSIBLE_REVISION

RUN git clone https://github.com/prius/ansible-playbooks.git \
        /etc/my/ansible/ && \
    cd /etc/my/ansible/ && \
    test "x${ANSIBLE_REVISION}" != "x" && git reset --hard ${ANSIBLE_REVISION} \
        || true

ADD files/etc/my/ansible/manual/ /etc/my/ansible/manual/

WORKDIR /etc/my/ansible/

RUN . /usr/lib/my/ansible/bin/activate; \
    ansible-playbook --diff -i hosts common.yml

RUN rm -rf /var/lib/apt/lists/ /var/cache/apt/

CMD ["bash", "-x", "./run.sh"]
