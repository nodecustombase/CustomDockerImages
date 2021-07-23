#nodealpine16-npm7_20_0-couchbase_imagemagick
FROM nodecustombase/nodealpine16-npm7_20_0
RUN apk add --no-cache make gcc g++ python3 curl \
    && curl -sfSLO https://dl-cdn.alpinelinux.org/alpine/v3.14/community/x86_64/imagemagick-7.0.11.13-r0.apk \
    && apk add --allow-untrusted imagemagick-7.0.11.13-r0.apk \
    && apk del curl
