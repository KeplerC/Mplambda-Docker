#  Copyright 2019 U.C. Berkeley RISE Lab
#
#  Licensed under the Apache License, Version 2.0 (the "License");
#  you may not use this file except in compliance with the License.
#  You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
#  Unless required by applicable law or agreed to in writing, software
#  distributed under the License is distributed on an "AS IS" BASIS,
#  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#  See the License for the specific language governing permissions and
#  limitations under the License.

FROM ubuntu:18.04


RUN apt-get update
RUN apt install -y sudo git wget libassimp-dev libcurl4-openssl-dev libspdlog-dev libcurl4-openssl-dev libeigen3-dev ninja-build
     
# cmake 

# Install Cmake
RUN wget "https://cmake.org/files/v3.15/cmake-3.15.4-Linux-x86_64.tar.gz" && \
    tar xvzf cmake-3.15.4-Linux-x86_64.tar.gz && \
    mv cmake-3.15.4-Linux-x86_64 /usr/bin/cmake && \
    rm /cmake-3.15.4-Linux-x86_64.tar.gz 
ENV PATH $PATH:/usr/bin/cmake/bin
RUN cmake --version 

#Libccd 
WORKDIR /
RUN git clone https://github.com/UNC-Robotics/nigh.git && \
    git clone https://github.com/danfis/libccd.git 
WORKDIR /libccd
RUN mkdir build && cd build && \
    cmake -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release .. && \
    make -j8 && make install


WORKDIR /
RUN git clone https://github.com/flexible-collision-library/fcl.git && \
    cd fcl && mkdir build && cd build && \
    cmake -DCMAKE_BUILD_TYPE=Release .. && make -j8 && make install  
    

WORKDIR /
RUN git clone https://github.com/BerkeleyAutomation/mplambda.git 


WORKDIR /mplambda
RUN mkdir -p build/ && \
    cd build/ && \
    cmake -DCMAKE_BUILD_TYPE=Release -DAPPLE=1 .. && \
    make -j8 
RUN mkdir -p build/

WORKDIR /

