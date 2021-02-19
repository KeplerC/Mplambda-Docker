
FROM alpine:3.10 

RUN apk add --update alpine-sdk  cmake musl-dev git

#RUN mkdir /home/git; \
#    cd /home/git; \
#    sudo git clone https://github.com/DavidArayan/assimp.git -b master; \
#    cd assimp; \
#    mkdir build; \
#    cd build; \
#    cmake CMakeLists.txt -G 'Unix Makefiles' ../; \
#    make

#RUN gcc -dM -E - < nul

RUN mkdir /home/git; \
    cd /home/git; \
    git clone https://github.com/assimp/assimp.git -b master; 

RUN cd /home/git/assimp; \
    cmake CMakeLists.txt -G 'Unix Makefiles'; \
    make; \
    make install; 
    #ldconfig;
RUN cp /home/git/assimp/bin/* /usr/local/bin/
RUN assimp help