FROM fedora:24

RUN dnf install -y gcc-c++ cmake make git openssl-devel
RUN git clone https://github.com/nats-io/cnats.git
RUN cd cnats && cmake . && make
