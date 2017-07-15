## libs-multi-libdirs

### Goal

Create a web app that has other lib dependencies packaged within the war file but
out of the standard classpath such that they can be managed with by other classloaders.
As there will be multiple groups of possibly incompatible libs the desire is to have the
 ability to partition into multiple groups and have Maven take care of resolving transitive
 dependencies within each.

### Approach

Maven multi-module project with dependency groups isolated within modules and then copied
into the web module during the project build.

### Notes

If I run `mvn dependency:tree` each module seems to have only their own dependencies listed and
after running `mvn package` they all end up packaged into the war.

An oddity I noticed was if you run `mvn clean package` (two goals instead of one) the clean and then package
is executed for each module so the copy of the libs gets wiped out when the web module is packaged. Running
one at a time gives the expected results.

## Results

```
web/target/web
├── META-INF
├── WEB-INF
│   ├── classes
│   └── web.xml
└── libs-ext
    ├── avro
    │   ├── avro-1.7.6.jar
    │   ├── commons-compress-1.4.1.jar
    │   ├── jackson-core-asl-1.9.13.jar
    │   ├── jackson-mapper-asl-1.9.13.jar
    │   ├── paranamer-2.3.jar
    │   ├── slf4j-api-1.6.4.jar
    │   ├── snappy-java-1.0.5.jar
    │   └── xz-1.0.jar
    └── solr
        ├── commons-io-2.5.jar
        ├── httpclient-4.4.1.jar
        ├── httpcore-4.4.1.jar
        ├── httpmime-4.4.1.jar
        ├── jcl-over-slf4j-1.7.7.jar
        ├── noggit-0.6.jar
        ├── slf4j-api-1.7.7.jar
        ├── solr-solrj-6.3.0.jar
        ├── stax2-api-3.1.4.jar
        ├── woodstox-core-asl-4.4.1.jar
        └── zookeeper-3.4.6.jar
```