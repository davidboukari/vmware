FROM docker.io/centos:7.9.2009
#EXPOSE 80 443

ADD extra-packages ./

RUN yum -y install $(<extra-packages)

RUN ( VERSION=0.13.5 && TARGET=terraform_${VERSION}_linux_amd64 && cd /root && wget  "https://releases.hashicorp.com/terraform/${VERSION}/${TARGET}.zip" && unzip "${TARGET}.zip" && rm "${TARGET}.zip" && chmod +x terraform &&  mv terraform  /usr/local/bin)

RUN ( VERSION=1.15.5 && cd /root && wget -O- https://golang.org/dl/go${VERSION}.linux-amd64.tar.gz|tar xzvf - && mv go /usr/local && echo "export PATH=$PATH:/usr/local/go/bin" >> /etc/bashrc )  

#ADD ./terraform-sample-1 /root/terraform-sample-1

RUN (export PATH=$PATH:/usr/local/go/bin && cd /root && git clone https://github.com/hashicorp/terraform-provider-vsphere.git && cd terraform-provider-vsphere && make) 

RUN ( cd /root && git clone https://github.com/davidboukari/terraform-vsphere-samples.git )

#VOLUME /sys/fs/cgroup
#VOLUME /run /tmp

