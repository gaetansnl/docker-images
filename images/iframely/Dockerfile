FROM node:12.10
RUN mkdir /app
RUN cd /app && git clone --branch v1.4.4 https://github.com/itteco/iframely.git && cd iframely && npm install && apt-get install libkrb5-dev
WORKDIR /app/iframely
CMD ["node", "server"]
