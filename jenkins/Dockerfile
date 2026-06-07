FROM jenkins/jenkins:lts-jdk17

USER root

COPY zscaler-ca.cer /tmp/zscaler-ca.cer
RUN keytool -import -trustcacerts \
  -keystore $JAVA_HOME/lib/security/cacerts \
  -storepass changeit -noprompt \
  -alias zscaler-root \
  -file /tmp/zscaler-ca.cer

RUN apt-get update && apt-get install -y lsb-release \
  && echo "insecure" >> /etc/curlrc \
  && echo 'Acquire::https::Verify-Peer "false";\nAcquire::https::Verify-Host "false";' > /etc/apt/apt.conf.d/99insecure

RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  --insecure \
  https://download.docker.com/linux/debian/gpg

RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  bookworm stable" > /etc/apt/sources.list.d/docker.list

RUN apt-get update && apt-get install -y docker-ce-cli

RUN git config --system http.sslVerify false

USER jenkins