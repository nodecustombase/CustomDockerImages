#nodealpine16
FROM alpine:3.14
ENV VERSION=v16.8.0 NPM_VERSION=7.23.0

RUN apk upgrade --no-cache -U && \
  apk add --no-cache curl make gcc g++ python3 linux-headers binutils-gold gnupg libstdc++ && \
  curl -sfSLO http://dl-cdn.alpinelinux.org/alpine/v3.14/main/x86_64/busybox-1.33.1-r3.apk  && \
  curl -sfSLO http://dl-cdn.alpinelinux.org/alpine/v3.14/main/x86_64/ssl_client-1.33.1-r3.apk && \
  curl -sfSLO https://dl-cdn.alpinelinux.org/alpine/v3.14/main/x86_64/apk-tools-2.12.7-r0.apk && \
  apk add --allow-untrusted busybox-1.33.1-r3.apk && \
  apk add --allow-untrusted ssl_client-1.33.1-r3.apk && \  
  apk add --allow-untrusted apk-tools-2.12.7-r0.apk && \  
   for key in \
      4ED778F539E3634C779C87C6D7062848A1AB005C \
      94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
      74F12602B6F1C4E913FAA37AD3A89613643B6201 \
      71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
      8FCCA13FEF1D0C2E91008E09770F7A9A5AE15600 \
      C4F0DFFF4E8C1A8236409D08E73BC641CC11F4C8 \
      C82FA3AE1CBEDC6BE46B9360C43CEC45C17AB93C \
      DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
      A48C2BEE680E841632CD4E44F07496B3EB3C1762 \
      108F52B48DB57BB0CC439B2997B01419BD92F80A \
      B9E2F5981AA6E0CD28160D9FF13993A75599653C \
    ; do \
      gpg --batch --keyserver hkps://keys.openpgp.org --recv-keys "$key" || \
      gpg --batch --keyserver keyserver.ubuntu.com --recv-keys "$key" ; \
    done && \
  curl -sfSLO https://nodejs.org/dist/${VERSION}/node-${VERSION}.tar.xz && \
  curl -sfSL https://nodejs.org/dist/${VERSION}/SHASUMS256.txt.asc | gpg -d -o SHASUMS256.txt && \
  grep " node-${VERSION}.tar.xz\$" SHASUMS256.txt | sha256sum -c | grep ': OK$' && \
  tar -xf node-${VERSION}.tar.xz && \
  rm -rf node-${VERSION}/deps/npm && \
  cd node-${VERSION} && \
  ./configure --prefix=/usr ${CONFIG_FLAGS} && \
  make -j$(getconf _NPROCESSORS_ONLN) && \
  make install

RUN apk del curl make gcc g++ python3 linux-headers binutils-gold gnupg ${DEL_PKGS} && \
  rm -rf ${RM_DIRS} /node-${VERSION}* /SHASUMS256.txt /tmp/* \
    /usr/share/man/* /usr/share/doc /root/.npm /root/.node-gyp /root/.config \
    /usr/lib/node_modules/npm/man /usr/lib/node_modules/npm/doc /usr/lib/node_modules/npm/docs \
    /usr/lib/node_modules/npm/html /usr/lib/node_modules/npm/scripts && \
  { rm -rf /root/.gnupg || true; }

COPY docker-entrypoint.sh /usr/local/bin/

ENTRYPOINT ["docker-entrypoint.sh"]

CMD [ "node" ]