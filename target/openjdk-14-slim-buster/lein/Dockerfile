FROM openjdk:14-slim-buster

ENV LEIN_VERSION=2.9.3
ENV LEIN_INSTALL=/usr/local/bin/

WORKDIR /tmp

# Download the whole repo as an archive
RUN \
apt-get update && \
apt-get install -y gnupg wget && \
rm -rf /var/lib/apt/lists/* && \
mkdir -p $LEIN_INSTALL && \
wget -q https://raw.githubusercontent.com/technomancy/leiningen/$LEIN_VERSION/bin/lein-pkg && \
echo "Comparing lein-pkg checksum ..." && \
sha256sum lein-pkg && \
echo "42e18e8a833b863ddfba1c5565bd5d78b54bcee661ec86e94a8bdc67b1733e63 *lein-pkg" | sha256sum -c - && \
mv lein-pkg $LEIN_INSTALL/lein && \
chmod 0755 $LEIN_INSTALL/lein && \
wget -q https://github.com/technomancy/leiningen/releases/download/$LEIN_VERSION/leiningen-$LEIN_VERSION-standalone.zip && \
wget -q https://github.com/technomancy/leiningen/releases/download/$LEIN_VERSION/leiningen-$LEIN_VERSION-standalone.zip.asc && \
gpg --batch --keyserver keys.openpgp.org --recv-key 20242BACBBE95ADA22D0AFD7808A33D379C806C3 && \
echo "Verifying file PGP signature..." && \
gpg --batch --verify leiningen-$LEIN_VERSION-standalone.zip.asc leiningen-$LEIN_VERSION-standalone.zip && \
rm leiningen-$LEIN_VERSION-standalone.zip.asc && \
mkdir -p /usr/share/java && \
mv leiningen-$LEIN_VERSION-standalone.zip /usr/share/java/leiningen-$LEIN_VERSION-standalone.jar && \
apt-get purge -y --auto-remove gnupg wget

ENV PATH=$PATH:$LEIN_INSTALL
ENV LEIN_ROOT 1

# Install clojure 1.10.1 so users don't have to download it every time
RUN echo '(defproject dummy "" :dependencies [[org.clojure/clojure "1.10.1"]])' > project.clj \
  && lein deps && rm project.clj

CMD ["lein", "repl"]