FROM node:10-slim

# add curl for healthcheck
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Add Tini for proper init of signals
ENV TINI_VERSION v0.19.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini

WORKDIR /app

# have nodemon available for local dev use (file watching)
RUN npm install -g nodemon

COPY package*.json ./

RUN npm i 

COPY . .

ENV PORT 4000

EXPOSE 4000

CMD ["/tini", "--", "node", "server.js"]
