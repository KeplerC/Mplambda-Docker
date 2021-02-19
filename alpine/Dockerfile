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

# alpine version comes from https://github.com/lsds/sgx-lkl/blob/oe_port/samples/languages/python/Dockerfile
FROM alpine:3.10 


RUN apk update 
RUN apk add --no-cache sudo git wget alpine-sdk  cmake musl-dev git

# https://github.com/dotnet/core/issues/226 for  libcurl4-openssl-dev
RUN apk add --no-cache curl-dev 



# assimp
# reference: https://github.com/bferns/assimp-alpine-docker-build/blob/master/Dockerfile
RUN git clone https://github.com/assimp/assimp.git -b master; 
WORKDIR /assimp
RUN cmake CMakeLists.txt -G 'Unix Makefiles'; \
    make; \
    make install; 
# for some reason, default installation is not successful
RUN cp /assimp/bin/* /usr/local/bin/ 

# eigen3
WORKDIR / 
RUN git clone https://github.com/eigenteam/eigen-git-mirror.git
RUN cd eigen-git-mirror && mkdir build && cd build && \
    cmake .. &&\
    make install
RUN apk add --no-cache eigen-dev

#Libccd 
WORKDIR /
RUN git clone https://github.com/UNC-Robotics/nigh.git && \
    git clone https://github.com/danfis/libccd.git 

WORKDIR /libccd
RUN mkdir build && cd build && \
    cmake -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release .. && \
    make -j8 && make install

# fcl
WORKDIR /
RUN git clone https://github.com/flexible-collision-library/fcl.git && \
    cd fcl && mkdir build && cd build && \
    cmake -DCMAKE_BUILD_TYPE=Release .. && make -j8 && make install  
    
# I need to use my own repo because the original cmake file does not support custom assimp
# if you need write access to my repo, let me know
WORKDIR /
RUN git clone https://github.com/KeplerC/mplambda.git 


# finally 
# the paths are pretty fragile, handle with care
WORKDIR /mplambda
RUN mkdir -p build/ && \
    cd build/ && \
    cmake -DCMAKE_BUILD_TYPE=Release -DAPPLE=1 .. -DAssimp_DIR="/usr/local/lib64/cmake/assimp-5.0" -DCMAKE_PREFIX_PATH="/usr/local;/usr/local/include;/usr/include;/usr/local/lib64/cmake/assimp-5.0" && \
    make -j8 

WORKDIR /

