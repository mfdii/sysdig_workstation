FROM ubuntu:17.10

RUN apt-get update && apt-get install -y ruby git curl python-pip && \
	apt-get clean

RUN curl -O https://packages.chef.io/files/stable/chefdk/2.4.17/ubuntu/16.04/chefdk_2.4.17-1_amd64.deb && \
	dpkg -i chefdk_2.4.17-1_amd64.deb && rm -f chefdk_2.4.17-1_amd64.deb
	
RUN pip install awscli 
	
RUN	curl -O https://releases.hashicorp.com/packer/1.1.3/packer_1.1.3_linux_amd64.zip && \
	unzip -oq packer_1.1.3_linux_amd64.zip && \
	mv packer /usr/local/bin && rm -f packer*.zip 


RUN git clone https://github.com/mfdii/sysdig_workstation

WORKDIR sysdig_workstation

RUN gem install bundler && rm -f Gemfile.lock && \
	bundle install && rake cookbook:vendor

CMD ["rake"]