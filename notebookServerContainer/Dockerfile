# ubuntu image
FROM ubuntu:18.04

# system updates
RUN apt-get -y update
RUN apt-get -y install vim # for interactive editing
RUN apt-get -y install python3.7
RUN apt-get -y install python3-pip

# bring in requirements.txt
RUN mkdir /requirements
COPY ./requirements.txt /requirements/

# install python requirements
WORKDIR /requirements
RUN pip3 install -r requirements.txt
RUN pip3 install jupyter

# set up jupyter server
# RUN jupyter notebook --generate-config

# from https://jupyter-notebook.readthedocs.io/en/stable/public_server.html
#   Add Tini. Tini operates as a process subreaper for jupyter. This prevents
#     kernel crashes.
ENV TINI_VERSION v0.6.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /usr/bin/tini
RUN chmod 111 /usr/bin/tini
#ENTRYPOINT ["/usr/bin/tini", "--"]

# create user
RUN groupadd -g 999 appuser && \
	useradd -r -u 999 -g appuser appuser
USER appuser


EXPOSE 8888
#CMD ["jupyter", "notebook", "--port=8888", "--no-browser", "--ip=0.0.0.0"]


