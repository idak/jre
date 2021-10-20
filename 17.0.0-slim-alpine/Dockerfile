FROM alpine:3.13.6 AS build

ENV JAVA_HOME /opt/jdk
ENV PATH $JAVA_HOME/bin:$PATH

# Download alpine jdk from azul using https://www.azul.com/downloads/?version=java-17-lts&os=alpine-linux&package=jdk
ADD https://cdn.azul.com/zulu/bin/zulu17.28.13-ca-jdk17.0.0-linux_musl_x64.tar.gz $JAVA_HOME/openjdk.tar.gz

# unzip openjdk file
RUN tar --extract --file $JAVA_HOME/openjdk.tar.gz --directory "$JAVA_HOME" --strip-components 1; \
	rm $JAVA_HOME/openjdk.tar.gz;

# Build custom jre with all java modules
RUN ALL_MODULES="$( echo $(java --list-modules) | sed 's/@17\s/,/g' | sed 's/@17/,/g')" \
    && jlink --compress=2 --module-path /opt/jdk/jmods --add-modules "$ALL_MODULES" --no-header-files --output jre

#------------------------------------------Build JRE docker image -------------------------------------------------
FROM alpine:3.13.6

# Copy generated JRE from image builder
COPY --from=build  /jre /opt/jdk

ENV JAVA_HOME /opt/jdk
ENV PATH=$PATH:/opt/jdk/bin

CMD ["java", "-version"]
