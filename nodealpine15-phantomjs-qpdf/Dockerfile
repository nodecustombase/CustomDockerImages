#nodealpine15-couchbase
FROM nodecustombase/nodealpine15
RUN apk add --no-cache make qpdf 
COPY ./phantomjs-2.1.1-linux-x86_64.tar.bz2 .

RUN tar xvjf phantomjs-2.1.1-linux-x86_64.tar.bz2 -C /usr/local/share/
RUN ln -s /usr/local/share/phantomjs-2.1.1-linux-x86_64/bin/phantomjs /usr/local/bin/phantomjs