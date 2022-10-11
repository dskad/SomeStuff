FROM eclipse-temurin:11-jdk-jammy
#FROM gradle:7.5-jdk18
# FROM ubuntu:22.04
# ENV JAVA_HOME=/opt/java/openjdk
# COPY --from=eclipse-temurin:19 $JAVA_HOME $JAVA_HOME
# ENV PATH="${JAVA_HOME}/bin:${PATH}"

ARG DEBIAN_FRONTEND=noninteractive

ARG CHROME_VERSION

RUN apt-get update && apt-get install -y curl \
  \
  && export CHROME_LATEST=$(curl -L -s https://dl.google.com/linux/chrome/deb/dists/stable/main/binary-amd64/Packages | awk '/google-chrome-stable/{x=1}x&&/Version:/{print $2;exit}') \
  \
  && curl -L -s https://dl-ssl.google.com/linux/linux_signing_key.pub -o /etc/apt/trusted.gpg.d/google.asc \
  && echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list \
  && apt-get update \
  \
  && apt-get -y --no-install-recommends install \
  google-chrome-stable=${CHROME_VERSION:=$CHROME_LATEST} \
  unzip \
  \
  && apt-get clean \
  && rm /etc/apt/sources.list.d/google-chrome.list \
  && rm -rf /var/lib/apt/lists/* /var/cache/apt/* \
  \
  && export CHROME_DRIVER_VERSION=$(curl -L -s https://chromedriver.storage.googleapis.com/LATEST_RELEASE_${CHROME_VERSION%.*}) \
  && curl -L -s -o /tmp/chromedriver.zip https://chromedriver.storage.googleapis.com/${CHROME_DRIVER_VERSION}/chromedriver_linux64.zip \
  && unzip /tmp/chromedriver.zip -d /opt \
  && rm /tmp/chromedriver.zip \
  && mv /opt/chromedriver /opt/chromedriver-$CHROME_DRIVER_VERSION \
  && chmod 755 /opt/chromedriver-$CHROME_DRIVER_VERSION \
  && ln -s /opt/chromedriver-$CHROME_DRIVER_VERSION /usr/bin/chromedriver \
  \
  && groupadd -r chrometest \
  && useradd --no-log-init -r -g chrometest chrometest \
  && mkdir /home/chrometest \
  && chown chrometest:chrometest /home/chrometest
  #&& sed -i 's/"$HERE\/chrome"/"$HERE\/chrome" --no-sandbox/g' /opt/google/chrome/google-chrome

  USER chrometest
  WORKDIR /home/chrometest
