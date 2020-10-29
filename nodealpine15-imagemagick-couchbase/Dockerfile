#nodealpine15-imagemagick-couchbase
FROM node:15-alpine
RUN apk add --no-cache make gcc g++ python3 graphicsmagick \
    && npm install -g couchbase \    
    && npm uninstall -g just-diff-apply && rm -rf /usr/local/lib/node_modules/npm/node_modules/just-diff-apply && ls /usr/local/lib/node_modules/npm/node_modules \
    && apk del --force --force-broken-world ghostscript-fonts