#nodealpine15-imagemagick-ffmpeg-couchbase
FROM nodecustombase/nodealpine15
RUN apk add --no-cache make gcc g++ python3 graphicsmagick ffmpeg \
    && npm install -g couchbase \
    && apk del --force --force-broken-world ghostscript-fonts
