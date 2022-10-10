FROM gradle:7.5-jdk18

ARG DEBIAN_FRONTEND=noninteractive

ARG CHROME_VERSION

RUN export CHROME_LATEST=$(curl -s https://dl.google.com/linux/chrome/deb/dists/stable/main/binary-amd64/Packages | awk '/google-chrome-stable/{x=1}x&&/Version:/{print;exit}' | cut -d ' ' -f2) \
  && wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | tee /etc/apt/trusted.gpg.d/google.asc \
  && echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list \
  && apt-get update \
  && apt-get -y --no-install-recommends install \
  google-chrome-stable=${CHROME_VERSION:=$CHROME_LATEST} \
  unzip \
  git \
  && export CHROME_DRIVER_VERSION=$(curl -s https://chromedriver.storage.googleapis.com/LATEST_RELEASE_${CHROME_VERSION%.*}) \
  && wget -q -O /tmp/chromedriver.zip https://chromedriver.storage.googleapis.com/${CHROME_DRIVER_VERSION}/chromedriver_linux64.zip \
  && unzip /tmp/chromedriver.zip -d /opt \
  && rm /tmp/chromedriver.zip \
  && mv /opt/chromedriver /opt/chromedriver-$CHROME_DRIVER_VERSION \
  && chmod 755 /opt/chromedriver-$CHROME_DRIVER_VERSION \
  && ln -s /opt/chromedriver-$CHROME_DRIVER_VERSION /usr/bin/chromedriver \
  && apt-get clean \
  && rm /etc/apt/sources.list.d/google-chrome.list \
  && rm -rf /var/lib/apt/lists/* /var/cache/apt/* \
  && sed -i 's/"$HERE\/chrome"/"$HERE\/chrome" --no-sandbox/g' /opt/google/chrome/google-chrome
