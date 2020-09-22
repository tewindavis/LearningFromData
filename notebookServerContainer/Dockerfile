# setup process user is root
ARG ROOT_USER="root"
ARG APP_USER="appuser"


# ubuntu image
FROM ubuntu:18.04


USER $ROOT_USER


# system updates
RUN apt-get -y update
RUN apt-get install -y vim \
	python3.7 \
	python3-pip \
	graphviz


# bring in requirements.txt
RUN mkdir /requirements
COPY ./requirements.txt /requirements/


# install python requirements
WORKDIR /requirements
RUN pip3 install -r requirements.txt
RUN pip3 install jupyter


# set up jupyter server
# RUN jupyter notebook --generate-config



# create user
RUN groupadd -g 999 appuser && \
	useradd -r -u 999 -g appuser appuser



# from https://jupyter-notebook.readthedocs.io/en/stable/public_server.html
#   Add Tini. Tini operates as a process subreaper for jupyter. This prevents
#     kernel crashes.
ENV TINI_VERSION v0.6.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /usr/bin/tini
RUN chmod 777 /usr/bin/tini

EXPOSE 8888
ENTRYPOINT ["/usr/bin/tini", "--"]

USER $APP_USER

CMD ["jupyter", "notebook", "--port=8888", "--no-browser", "--ip=0.0.0.0", "--allow-root"]


