FROM ubuntu:16.04
RUN apt-get update && \
    apt-get install -y wget \
                       sudo \
                       curl \
                       git

RUN wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb
RUN apt-get update && \
     apt-get install -y apt-transport-https
RUN sudo dpkg -i packages-microsoft-prod.deb
RUN apt-get install -y software-properties-common && \
    apt-get update
RUN sudo add-apt-repository universe
RUN sudo apt-get install -y apt-transport-https \
                            dotnet-sdk-3.0
 
RUN curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
