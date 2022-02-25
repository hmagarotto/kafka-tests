FROM docker.io/bitnami/kafka:1.1.0

USER root

# https://github.com/prometheus/jmx_exporter
ADD https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/0.16.1/jmx_prometheus_javaagent-0.16.1.jar /opt/bitnami/kafka/libs/jmx_prometheus_javaagent-0.16.1.jar
ADD https://raw.githubusercontent.com/prometheus/jmx_exporter/master/example_configs/kafka-0-8-2.yml /opt/bitnami/kafka/conf/jmx_prometheus.yml

RUN chmod 664 /opt/bitnami/kafka/libs/jmx_prometheus_javaagent-0.16.1.jar
RUN chmod 664 /opt/bitnami/kafka/conf/jmx_prometheus.yml

USER 1001

ENV KAFKA_OPTS="${KAFKA_OPTS} -javaagent:/opt/bitnami/kafka/libs/jmx_prometheus_javaagent-0.16.1.jar=9095:/opt/bitnami/kafka/config/jmx_prometheus.yml"
