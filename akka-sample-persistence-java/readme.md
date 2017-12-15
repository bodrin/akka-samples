* forked https://github.com/akka/akka-samples into https://github.com/bodrin/akka-samples
* removed akka-samples/akka-sample-persistence-java/src/main/resources/application.conf
* copied akka-persistence-jdbc/src/main/resources/reference.conf into akka-samples/akka-sample-persistence-java/src/main/resources/application.conf

//* copied akka-persistence-jdbc/src/test/resources/* from https://github.com/dnvriend/akka-persistence-jdbc into akka-samples/akka-sample-persistence-java/src/main/resources/ 

* added to pom.xml:

        <dependency>
            <groupId>com.typesafe.akka</groupId>
            <artifactId>akka-slf4j_${scalaBinaryVersion}</artifactId>
            <version>2.5.4</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-simple</artifactId>
            <version>1.7.23</version>
        </dependency>

* created akka-plugin user and akka-plugin DB in PostgreSQL
* psql -h localhost akka-plugin akka-plugin

DROP TABLE IF EXISTS public.journal;

CREATE TABLE IF NOT EXISTS public.journal (
  ordering BIGSERIAL,
  persistence_id VARCHAR(255) NOT NULL,
  sequence_number BIGINT NOT NULL,
  deleted BOOLEAN DEFAULT FALSE,
  tags VARCHAR(255) DEFAULT NULL,
  message BYTEA NOT NULL,
  PRIMARY KEY(persistence_id, sequence_number)
);

CREATE UNIQUE INDEX journal_ordering_idx ON public.journal(ordering);

DROP TABLE IF EXISTS public.snapshot;

CREATE TABLE IF NOT EXISTS public.snapshot (
  persistence_id VARCHAR(255) NOT NULL,
  sequence_number BIGINT NOT NULL,
  created BIGINT NOT NULL,
  snapshot BYTEA NOT NULL,
  PRIMARY KEY(persistence_id, sequence_number)
);

above taken from https://github.com/dnvriend/akka-persistence-jdbc/blob/master/src/test/resources/schema/postgres/postgres-schema.sql

