FROM ubuntu:focal

ENV WS_DIR="/benchmark"
WORKDIR ${WS_DIR}

SHELL ["/bin/bash", "-c"]

ARG DEBIAN_FRONTEND=noninteractive

# Dependencies for latency plot
RUN apt-get update && apt-get install -y \
    curl \
    grep \
    build-essential \
    make \
    libnuma-dev \
    python3 \
    python3-distutils \
    gnuplot-qt
# Dependencies for Bash unit-tests
RUN apt-get update && apt-get install -y \
    bats \
    dialog \
    tmux

# Fetch most recent version of rt-tests and mklatencyplot
RUN cd ${WS_DIR} \
 && RT_TESTS=$(echo $(curl -Ls https://mirrors.edge.kernel.org/pub/linux/utils/rt-tests | grep -o -P '(?<=href\=\")(rt-tests-2.*)(?=.tar.xz\">)')) \
 && curl -SLO --fail https://mirrors.edge.kernel.org/pub/linux/utils/rt-tests/${RT_TESTS}.tar.xz \
 && tar -xf ${RT_TESTS}.tar.xz \
 && cd ${RT_TESTS} \
 && make \
 && make install \
 && cd .. \
 && rm -r ${RT_TESTS} \
 && rm ${RT_TESTS}.tar.xz \
 && curl -SLO --fail https://www.osadl.org/uploads/media/mklatencyplot.bash \
# && sed -i -E "s/(cores=).*/\1\$(nproc)/" ${WS_DIR}/mklatencyplot.bash \
 && chmod +x mklatencyplot.bash

ARG DEBIAN_FRONTEND=dialog

