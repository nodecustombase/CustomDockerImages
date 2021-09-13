#nodealpine16-no-npm7_20_0-qpdf-phantomjs
FROM sum1else/nodealpine16-no-npm7_20_0-qpdf

COPY dockerized-phantomjs.tar.gz ./
COPY phantomjs-2.1.1-linux-x86_64.tar.bz2 ./

RUN tar -zxvf ./dockerized-phantomjs.tar.gz -C ./ \
    && rm -f ./dockerized-phantomjs.tar.gz

RUN tar -jxvf ./phantomjs-2.1.1-linux-x86_64.tar.bz2 -C ./ \
    && cp ./phantomjs-2.1.1-linux-x86_64/bin/phantomjs ./usr/bin/phantomjs \
    && rm -f ./phantomjs-2.1.1-linux-x86_64.tar.bz2 \
    && phantomjs --version

RUN rm -f ./dockerized-phantomjs.tar.gz \
    && rm -f ./phantomjs-2.1.1-linux-x86_64.tar.bz2