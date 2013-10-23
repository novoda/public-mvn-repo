======================================
Novoda Public Maven Repository
======================================

- This is a repository for open source Maven Projects available from Novoda.
- We also use it to release SNAPSHOT's so excuse the mess.

![img](https://encrypted-tbn1.gstatic.com/images?q=tbn:ANd9GcSbT61gtbI8vDDV6phYN5ztFurC2Nm5kSaziPCtwfF76GX4Q4Nb)

======================================
Use the repo with maven
======================================

if you want to use the jars in this repository you need to set up a profile of maven in ~/.m2/settings.xml
to do that simply copy the file in the public-mvn-repo/poms/settings.xml

======================================
Manually download of files
======================================

go to the file from github
change the url bit "blob" to "raw"

example :
from
https://github.com/novoda/public-mvn-repo/blob/master/releases/com/novoda/httpservice/core/1.2/core-1.2.jar
to
https://github.com/novoda/public-mvn-repo/raw/master/releases/com/novoda/httpservice/core/1.2/core-1.2.jar

[Making-Releases-Available](https://github.com/novoda/public-mvn-repo/wiki/Making-a-Project-Available)

[Releasing-a-Jar](https://github.com/novoda/public-mvn-repo/wiki/Releasing-a-Jar)


