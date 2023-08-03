# Java Shaded Dependency Example Project

a sample java/maven project with shaded dependencies. useful for testing whether SBOM and vulnerability tools detect the shaded dependencies.

if your tool fails to detect these dependencies, don't invite me to your party.

## shaded dependencies

these are shaded by the dependency [apache pulsar-client](https://github.com/apache/pulsar).

the dependencies are successfully detected in this project using [OWASP dependency-check](https://github.com/jeremylong/DependencyCheck). [its report](./dependency-check-report.html) is committed to this repo, but can be generated on demand by running `mvn clean verify`

the shaded dependencies, several of which have known vulnerabilities, are:

| Dependency                                                     | Known Vulnerabilities |
| -------------------------------------------------------------- | :-------------------: |
| com.google.protobuf:protobuf-java:2.4.1                        |          ✅           |
| com.fasterxml.jackson.core:jackson-core:2.9.9                  |                       |
| com.fasterxml.jackson.core:jackson-databind:2.9.9.3            |          ✅           |
| com.fasterxml.jackson.dataformat:jackson-dataformat-yaml:2.9.9 |                       |
| com.fasterxml.jackson.module:jackson-module-jsonSchema:2.9.9   |                       |
| com.google.code.gson:gson:2.8.2                                |          ✅           |
| com.google.guava:guava:25.1-jre                                |          ✅           |
| com.thoughtworks.paranamer:paranamer:2.7                       |                       |
| com.typesafe.netty:netty-reactive-streams:2.0.0                |                       |
| com.yahoo.datasketches:memory:0.8.3                            |                       |
| com.yahoo.datasketches:sketches-core:0.8.3                     |                       |
| commons-codec:commons-codec:1.10                               |                       |
| commons-configuration:commons-configuration:1.10               |                       |
| commons-lang:commons-lang:2.6                                  |                       |
| commons-logging:commons-logging:1.1.1                          |                       |
| io.netty:netty-all:4.1.32.Final                                |          ✅           |
| io.netty:netty-tcnative-boringssl-static:2.0.20.Final          |                       |
| io.swagger:swagger-annotations:1.5.21                          |                       |
| org.apache.avro:avro-protobuf:1.8.2                            |          ✅           |
| org.apache.avro:avro:1.8.2                                     |          ✅           |
| org.apache.bookkeeper:bookkeeper-common-allocator:4.9.2        |          ✅           |
| org.apache.commons:commons-compress:1.19                       |          ✅           |
| org.apache.commons:commons-lang3:3.4                           |                       |
| org.apache.httpcomponents:httpclient:4.5.5                     |          ✅           |
| org.apache.httpcomponents:httpcore:4.4.9                       |                       |
| org.apache.pulsar:pulsar-common:2.4.2                          |          ✅           |
| org.asynchttpclient:async-http-client-netty-utils:2.7.0        |                       |
| org.asynchttpclient:async-http-client:2.7.0                    |                       |
| org.eclipse.jetty:jetty-util:9.4.20.v20190813                  |          ✅           |
| org.yaml:snakeyaml:1.23                                        |          ✅           |

## output of `mvn dependency:tree`

as shown below, the shaded dependencies will not appear in the output of `mvn dependency:tree`.

this means that any SBOM or vulnerability scanners that only rely on the `mvn` output will miss these dependencies, and worse, will not identify vulnerable packages

```mvn
$ mvn dependency:tree
[INFO] Scanning for projects...
[WARNING]
[WARNING] Some problems were encountered while building the effective model for com.mycompany.app:my-app:jar:1.0-SNAPSHOT
[WARNING] 'build.plugins.plugin.version' for org.owasp:dependency-check-maven is missing. @ line 38, column 15
[WARNING]
[WARNING] It is highly recommended to fix these problems because they threaten the stability of your build.
[WARNING]
[WARNING] For this reason, future Maven versions might no longer support building such malformed projects.
[WARNING]
[INFO]
[INFO] ----------------------< com.mycompany.app:my-app >----------------------
[INFO] Building my-app 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ my-app ---
[INFO] com.mycompany.app:my-app:jar:1.0-SNAPSHOT
[INFO] +- junit:junit:jar:4.11:test
[INFO] |  \- org.hamcrest:hamcrest-core:jar:1.3:test
[INFO] \- org.apache.pulsar:pulsar-client:jar:2.4.2:compile
[INFO]    +- org.apache.pulsar:pulsar-client-api:jar:2.4.2:compile
[INFO]    +- org.apache.pulsar:protobuf-shaded:jar:2.1.0-incubating:compile
[INFO]    +- com.google.code.findbugs:jsr305:jar:3.0.2:compile
[INFO]    +- org.checkerframework:checker-qual:jar:2.0.0:compile
[INFO]    +- com.google.errorprone:error_prone_annotations:jar:2.1.3:compile
[INFO]    +- com.google.j2objc:j2objc-annotations:jar:1.1:compile
[INFO]    +- org.codehaus.mojo:animal-sniffer-annotations:jar:1.14:compile
[INFO]    +- org.lz4:lz4-java:jar:1.5.0:compile
[INFO]    +- com.github.luben:zstd-jni:jar:1.3.7-3:compile
[INFO]    +- org.bouncycastle:bcpkix-jdk15on:jar:1.60:compile
[INFO]    +- org.bouncycastle:bcprov-jdk15on:jar:1.60:compile
[INFO]    +- com.sun.activation:javax.activation:jar:1.2.0:compile
[INFO]    +- org.slf4j:slf4j-api:jar:1.7.25:compile
[INFO]    \- javax.validation:validation-api:jar:1.1.0.Final:compile
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 0.898 s
[INFO] Finished at: 2023-08-03T10:21:11-05:00
[INFO] ------------------------------------------------------------------------
```
