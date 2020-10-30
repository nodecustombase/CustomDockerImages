#nodealpine15-imagemagick-couchbase
FROM nodecustombase/nodealpine15
RUN apk add --no-cache make gcc g++ python3 graphicsmagick \
    && npm install -g couchbase \
    && apk del --force --force-broken-world ghostscript-fonts