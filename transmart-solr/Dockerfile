FROM openjdk:8
MAINTAINER Denny Verbeeck <dverbeec@its.jnj.com>

RUN apt-get update && apt-get install -y --no-install-recommends curl unzip make ca-certificates
# This is the latest commit at November 9th 2016, the week after the release of eTRIKS v3.0
RUN curl -L https://github.com/transmart/transmart-data/archive/d72385fca7b68011524aa12d9d0e7e1478e7e68a.zip -o transmart-data.zip && \
    unzip transmart-data.zip && \
    mv transmart-data-d72385fca7b68011524aa12d9d0e7e1478e7e68a transmart-data && \
    rm transmart-data.zip

WORKDIR /transmart-data

ENV TABLESPACES=dummy

RUN apt-get install -y --no-install-recommends rsync patch php5-cli php5-json 
    
RUN make -C solr solr-4.5.0.tgz
    
ENV PGHOST=tmdb
ENV PGPORT=5432
ENV PGDATABASE=transmart
    
RUN make -C solr solr_home
RUN apt-get purge -y --auto-remove curl unzip make rsync patch php5-cli php5-json ca-certificates && \
    rm -rf /var/lib/apt/lists/*

VOLUME ["/transmart-data/solr"]
EXPOSE 8983
WORKDIR /transmart-data/solr
ENTRYPOINT ["java", "-jar", "start.jar"]
    