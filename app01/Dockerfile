ARG BASE_IMAGE=amazoncorretto:11
FROM $BASE_IMAGE

RUN yum upgrade -y ;                                                            \
    yum install -y shadow-utils ;                                               \
    jq --version || yum install jq -y ;                                         \
    groupadd -r app -g 1042 &&                                                  \
    useradd -u 1042 -r -g app -m -d /app -s /sbin/nologin -c "App user" app &&  \
    chown -R app: /app &&                                                       \
    yum erase shadow-utils -y && yum clean all && rm -rfv /var/cache/yum

ARG JAR_FILE
ENV JAR_FILE $JAR_FILE
COPY --chown=app:app $JAR_FILE /app/$JAR_FILE
ENV JAVA_DEFAULT_OPTS="-XX:+PrintFlagsFinal"
WORKDIR /app
USER app
CMD echo "${JAVA_OPTS:-$JAVA_DEFAULT_OPTS}" && java ${JAVA_OPTS:-$JAVA_DEFAULT_OPTS} -jar /app/$JAR_FILE
