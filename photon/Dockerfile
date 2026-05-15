FROM eclipse-temurin:21-jre

WORKDIR /photon

RUN apt-get update -qq && \
    apt-get install -y --no-install-recommends wget bzip2 ca-certificates && \
    rm -rf /var/lib/apt/lists/*

# Download the latest Photon jar from GitHub releases
RUN wget -qO /tmp/release.json \
        "https://api.github.com/repos/komoot/photon/releases/latest" && \
    DOWNLOAD_URL=$(grep -o '"browser_download_url": "[^"]*\.jar"' /tmp/release.json | \
        head -1 | cut -d'"' -f4) && \
    echo "Downloading Photon: $DOWNLOAD_URL" && \
    wget -q "$DOWNLOAD_URL" -O /photon/photon.jar && \
    rm /tmp/release.json

RUN groupadd --gid 1001 photon && \
    useradd --uid 1001 --gid photon --shell /bin/bash --create-home photon && \
    chown photon:photon /photon/photon.jar

COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

VOLUME ["/photon/photon_data"]
EXPOSE 2322

ENTRYPOINT ["/entrypoint.sh"]
