FROM amazonlinux
RUN yum install -y yum-utils
RUN yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
RUN yum install -y tar unzip zip gcc-c++ libcurl-devel zlib-devel openssl-devel cmake3 git terraform

RUN arch=$(uname -m); curl https://awscli.amazonaws.com/awscli-exe-linux-${arch}.zip -o awscliv2.zip
RUN unzip awscliv2.zip
RUN ./aws/install
RUN rm awscliv2.zip
RUN rm -r aws

WORKDIR /root
RUN git clone https://github.com/awslabs/aws-lambda-cpp.git
WORKDIR /root/aws-lambda-cpp
RUN mkdir build
WORKDIR /root/aws-lambda-cpp/build
RUN cmake3 .. -DCMAKE_BUILD_TYPE=Release -DBUILD_SHARED_LIBS=OFF \
  -DCMAKE_INSTALL_PREFIX=~/out
RUN make && make install

WORKDIR /root
RUN git clone --recurse-submodules -j8 https://github.com/aws/aws-sdk-cpp.git
WORKDIR /root/aws-sdk-cpp
RUN mkdir build
WORKDIR /root/aws-sdk-cpp/build
RUN cmake3 .. -DBUILD_ONLY="s3;dynamodb" \
  -DBUILD_SHARED_LIBS=OFF \
  -DENABLE_UNITY_BUILD=ON \
  -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_INSTALL_PREFIX=~/out
RUN make && make install
