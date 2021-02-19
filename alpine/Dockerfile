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

# alpine version from https://github.com/lsds/sgx-lkl/blob/oe_port/samples/languages/python/Dockerfile
FROM alpine:3.10 


# RUN apt-get update
RUN apk update 
RUN apk add --no-cache sudo git wget alpine-sdk  cmake musl-dev git
# https://github.com/dotnet/core/issues/226 for  libcurl4-openssl-dev
RUN apk add --no-cache curl-dev 
#RUN apk add ninja-build 

#TODO:  spdlog 


# cmake 

# # Install Cmake
# RUN wget "https://cmake.org/files/v3.15/cmake-3.15.4-Linux-x86_64.tar.gz" && \
#     tar xvzf cmake-3.15.4-Linux-x86_64.tar.gz && \
#     mv cmake-3.15.4-Linux-x86_64 /usr/bin/cmake && \
#     rm /cmake-3.15.4-Linux-x86_64.tar.gz 
# ENV PATH $PATH:/usr/bin/cmake/bin
# RUN cmake --version 

# assimp
# reference: https://github.com/bferns/assimp-alpine-docker-build/blob/master/Dockerfile
RUN git clone https://github.com/assimp/assimp.git -b master; 
WORKDIR /assimp
RUN cmake CMakeLists.txt -G 'Unix Makefiles'; \
    make; \
    make install; 


#Libccd 
WORKDIR /
RUN git clone https://github.com/UNC-Robotics/nigh.git && \
    git clone https://github.com/danfis/libccd.git 

RUN apk add --no-cache eigen-dev


WORKDIR /libccd
RUN mkdir build && cd build && \
    cmake -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release .. && \
    make -j8 && make install


WORKDIR /
RUN git clone https://github.com/flexible-collision-library/fcl.git && \
    cd fcl && mkdir build && cd build && \
    cmake -DCMAKE_BUILD_TYPE=Release .. && make -j8 && make install  
    

WORKDIR /
RUN git clone https://github.com/KeplerC/mplambda.git 


# for some reason, default installation is not successful
RUN cp /assimp/bin/* /usr/local/bin/ 

WORKDIR / 
RUN git clone https://github.com/eigenteam/eigen-git-mirror.git
RUN cd eigen-git-mirror && mkdir build && cd build && \
    cmake .. &&\
    make install


WORKDIR /mplambda
RUN mkdir -p build/ && \
    cd build/ && \
    cmake -DCMAKE_BUILD_TYPE=Release -DAPPLE=1 .. -DAssimp_DIR="/usr/local/lib64/cmake/assimp-5.0" -DCMAKE_PREFIX_PATH="/usr/local;/usr/local/include;/usr/include;/usr/local/lib64/cmake/assimp-5.0" && \
    make -j8 
    
WORKDIR /

