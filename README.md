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