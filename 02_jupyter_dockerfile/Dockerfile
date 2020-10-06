# How to build: $ docker build -t ardihikaru/jupyter:1.0 .
# How to run:
#   1. CPU version
#       $ docker run --name my-jupyter -d -p 8888:8888 -v /home/<desired_path>/src/:/src/ ardihikaru/jupyter:1.0
#   2. GPU version
#       $ docker run --runtime=nvidia --name my-jupyter -d -p 8888:8888 -v /home/<desired_path>/src/:/src/ ardihikaru/jupyter:1.0
# How to enter container:
#   $ docker exec -it my-jupyter bash
FROM ardihikaru/nvidia-gpu-opencv:1.0
MAINTAINER Muhammad Febrian Ardiansyah (mfardiansyah.eed08g@nctu.edu.tw)

ENV LANG C.UTF-8
ENV DEBIAN_FRONTEND=noninteractive

RUN mkdir src
WORKDIR /src/notebooks

# Add Tini. Tini operates as a process subreaper for jupyter. This prevents kernel crashes.
ENV TINI_VERSION v0.6.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /usr/bin/tini
RUN chmod +x /usr/bin/tini
ENTRYPOINT ["/usr/bin/tini", "--"]

CMD ["jupyter", "notebook", "--port=8888", "--no-browser", "--ip=0.0.0.0", "--allow-root"]


