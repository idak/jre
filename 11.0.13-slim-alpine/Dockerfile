FROM alpine:3.13.6 AS build

ENV JAVA_HOME /opt/jdk
ENV PATH $JAVA_HOME/bin:$PATH

# Download alpine jdk from azul
ADD https://cdn.azul.com/zulu/bin/zulu11.52.13-ca-jdk11.0.13-linux_musl_x64.tar.gz $JAVA_HOME/openjdk.tar.gz

# unzip openjdk file
RUN tar --extract --file $JAVA_HOME/openjdk.tar.gz --directory "$JAVA_HOME" --strip-components 1; \
	rm $JAVA_HOME/openjdk.tar.gz;

# Build custom jre with all java modules
RUN ALL_MODULES="$( echo $(java --list-modules) | sed 's/@11.0.13\s/,/g' | sed 's/,open//g')" \
    && jlink --compress=2 --module-path /opt/jdk/jmods --add-modules "$ALL_MODULES" --no-header-files --output jre

#------------------------------------------Build JRE docker image -------------------------------------------------
FROM alpine:3.13.6

# Copy generated JRE from image builder
COPY --from=build  /jre /opt/jdk

ENV JAVA_HOME /opt/jdk
ENV PATH=$PATH:/opt/jdk/bin

CMD ["java", "-version"]
