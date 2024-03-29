#nodealpine15
FROM alpine:3.13.4
ENV VERSION=v15.13.0 NPM_VERSION=7.8.0 YARN_VERSION=v1.22.10

RUN apk upgrade --no-cache -U && \
  apk add --no-cache curl make gcc g++ python3 linux-headers binutils-gold gnupg libstdc++ && \
  curl -sfSLO http://dl-cdn.alpinelinux.org/alpine/v3.13/main/x86_64/busybox-1.32.1-r5.apk && \
  curl -sfSLO http://dl-cdn.alpinelinux.org/alpine/v3.13/main/x86_64/ssl_client-1.32.1-r5.apk && \
  apk add --allow-untrusted busybox-1.32.1-r5.apk && \
  apk add --allow-untrusted ssl_client-1.32.1-r5.apk && \  

 for server in ipv4.pool.sks-keyservers.net keyserver.pgp.com ha.pool.sks-keyservers.net; do \
    gpg --keyserver $server --recv-keys \
      4ED778F539E3634C779C87C6D7062848A1AB005C \
      94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
      71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
      8FCCA13FEF1D0C2E91008E09770F7A9A5AE15600 \
      C4F0DFFF4E8C1A8236409D08E73BC641CC11F4C8 \
      C82FA3AE1CBEDC6BE46B9360C43CEC45C17AB93C \
      DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
      A48C2BEE680E841632CD4E44F07496B3EB3C1762 \
      108F52B48DB57BB0CC439B2997B01419BD92F80A \
      B9E2F5981AA6E0CD28160D9FF13993A75599653C && break; \
  done && \

  curl -sfSLO https://nodejs.org/dist/${VERSION}/node-${VERSION}.tar.xz && \
  curl -sfSL https://nodejs.org/dist/${VERSION}/SHASUMS256.txt.asc | gpg -d -o SHASUMS256.txt && \
  grep " node-${VERSION}.tar.xz\$" SHASUMS256.txt | sha256sum -c | grep ': OK$' && \
  tar -xf node-${VERSION}.tar.xz && \
  curl -sfSLO https://github.com/npm/cli/archive/v${NPM_VERSION}.tar.gz && \
  tar -xf v${NPM_VERSION}.tar.gz && \
  rm -rf node-${VERSION}/deps/npm && \
  cp -r ./cli-${NPM_VERSION} node-${VERSION}/deps/npm && \
  ls -l node-${VERSION}/deps/npm && \
  cat node-${VERSION}/deps/npm/package.json && \
  cd node-${VERSION} && \
  ./configure --prefix=/usr ${CONFIG_FLAGS} && \
  make -j$(getconf _NPROCESSORS_ONLN) && \
  make install


RUN if [ -z "$CONFIG_FLAGS" ]; then \
    if [ -n "$NPM_VERSION" ]; then \
      npm install -g npm@${NPM_VERSION}; \
    fi; \
    find /usr/lib/node_modules/npm -type d \( -name test -o -name .bin \) | xargs rm -rf; \
    if [ -n "$YARN_VERSION" ]; then \
      for server in ipv4.pool.sks-keyservers.net keyserver.pgp.com ha.pool.sks-keyservers.net; do \
        gpg --keyserver $server --recv-keys \
          6A010C5166006599AA17F08146C2130DFD2497F5 && break; \
      done && \
      curl -sfSL -O https://github.com/yarnpkg/yarn/releases/download/${YARN_VERSION}/yarn-${YARN_VERSION}.tar.gz -O https://github.com/yarnpkg/yarn/releases/download/${YARN_VERSION}/yarn-${YARN_VERSION}.tar.gz.asc && \
      gpg --batch --verify yarn-${YARN_VERSION}.tar.gz.asc yarn-${YARN_VERSION}.tar.gz && \
      mkdir /usr/local/share/yarn && \
      tar -xf yarn-${YARN_VERSION}.tar.gz -C /usr/local/share/yarn --strip 1 && \
      ln -s /usr/local/share/yarn/bin/yarn /usr/local/bin/ && \
      ln -s /usr/local/share/yarn/bin/yarnpkg /usr/local/bin/ && \
      rm yarn-${YARN_VERSION}.tar.gz*; \
    fi; \
  fi

RUN apk del curl make gcc g++ python3 linux-headers binutils-gold gnupg ${DEL_PKGS} && \
  rm -rf ${RM_DIRS} /node-${VERSION}* /SHASUMS256.txt /tmp/* \
    /usr/share/man/* /usr/share/doc /root/.npm /root/.node-gyp /root/.config \
    /usr/lib/node_modules/npm/man /usr/lib/node_modules/npm/doc /usr/lib/node_modules/npm/docs \
    /usr/lib/node_modules/npm/html /usr/lib/node_modules/npm/scripts && \
  { rm -rf /root/.gnupg || true; }

COPY docker-entrypoint.sh /usr/local/bin/

RUN npm -v

ENTRYPOINT ["docker-entrypoint.sh"]

CMD [ "node" ]