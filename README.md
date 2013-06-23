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

======================================
Making releases available to others
======================================

The following steps will help you in publishing a new relase of a project onto this repo. 

***NOTE*** You will only need to do steps 1 and 2 once for your machine, 3 and 4 once for each project setup.

1: Clone this repo

2: Add this to your settings file in .m2/settings.xml, changing the <local.public.mvn.repo> to point to where you cloned this repo on your system

	<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                      http://maven.apache.org/xsd/settings-1.0.0.xsd">
	  <profiles>
	    <profile>
	      <id>localrelease</id>
	        <properties>
			  <local.public.mvn.repo>file:/Users/Peter/github/public-mvn-repo/releases</local.public.mvn.repo>
			</properties>
		</profile>
	  </profiles>

	  <activeProfiles>
		<activeProfile>localrelease</activeProfile>
	  </activeProfiles>
	</settings>




3: Add the following to the parent pom.xml of your project BUT change the github project url:

	<scm>
	    <url>http://github.com/novoda/Commons/tree/${scm.branch}</url>
	    <connection>scm:git:git://github.com/novoda/Commons.git</connection>
	    <developerConnection>scm:git:ssh://git@github.com/novoda/Commons.git</developerConnection>
	  </scm>

	<distributionManagement>
	    <repository>
	      <id>local-public-mvn-repo</id>
	      <name>local clone of https://github.com/novoda/public-mvn-repo</name>
	      <url>${local.public.mvn.repo}</url>
	    </repository>
	</distributionManagement>

	<build>
	    <plugins>
	      <plugin>
	        <artifactId>maven-scm-plugin</artifactId>
	        <configuration>
	          <scmVersionType>branch</scmVersionType>
	          <scmVersion>${scm.branch}</scmVersion>
	        </configuration>
	      </plugin>
	      <plugin>
	        <artifactId>maven-release-plugin</artifactId>
	        <configuration>
	          <autoVersionSubmodules>true</autoVersionSubmodules>
	          <useReleaseProfile>false</useReleaseProfile>
	        </configuration>
	        <goals />
	      </plugin>
	    </plugins>
	</build>


4: Ensure all modules in your project have their version on SNAPSHOT

5: Run this command from the root of your project BUT ensure everything is commited and pushed upstream from your local repo:

	mvn release:prepare release:perform -Plocalrelease

(This will run the tests in your prject, increment the version and publish a jar in your local public-mvn-repo repo)

6: Commit and push the changes in public-mvn-repo on your machine

======================================
Making local releases 
======================================

Run this command from the projects target/checkout/ directory:

	mvn clean install deploy:deploy -Plocalrelease  

======================================
Manually deploy a jar in the repo
======================================

1. clone the public-maven-repo
2. run the command

	<pre><code>
	 mvn deploy:deploy-file 
	 -DgroupId=com.your.group.id 
	 -DartifactId=your-artifact-id 
	 -Dversion=1.0.0.0 
	 -Dpackaging=jar 
	 -Dfile=path/to/file/jar_name.jar 
	 -Durl=file://path/to/local/cloned/repo/public-mvn-repo/releases/
	</code></pre>
	
	jar example
	<pre><code>
	  mvn deploy:deploy-file 
	  -DgroupId=com.omniture 
	  -DartifactId=omniture 
	  -Dversion=1.0.0 
	  -Dpackaging=jar 
	  -Dfile=omniture_app_measurement.jar 
	  -Durl=file:////Users/Blundell/Developer/git_repo/public-mvn-repo/releases/
	</code></pre>
	
	jar source example
	<pre><code>
	 mvn deploy:deploy-file 
	 -DgroupId=com.novoda 
	 -DartifactId=sexp 
	 -Dversion=1.0.0-SNAPSHOT 
	 -Dpackaging=java-source 
	 -Dfile=path/to/file/jar_name-1.0.0-javadoc.jar 
	 -Durl=file://path/to/local/cloned/repo/public-mvn-repo/releases/ 
	 -DgeneratePom=false
	</code></pre>
	
	pom example 
	<pre><code>
	 mvn deploy:deploy-file 
	 -DgroupId=com.novoda 
	 -DartifactId=reporting 
	 -Dversion=1.1.0 -Dpackaging=pom 
	 -Dfile=poms/reporting/pom.xml 
	 -Durl=file:///Users/Peter/github/public-mvn-repo/releases/
	</code></pre>
	
	apklib example (Adding pomFile parameter with location of relevant pom is important otherwise deployed pom will be broken due to missing dependencies. Also you will need to release parent pom in order to get apklib working, or you get error with parent not resolved)
	<pre><code>
	  mvn deploy:deploy-file -Dfile=/Users/Peter/github/ShowcaseView/library/target/showcaseview-library-3.1.apklib 
	  -DpomFile=/Users/Peter/github/ShowcaseView/library/pom.xml 
	  -DgroupId=com.github.espiandev 
	  -DartifactId=showcaseview-library 
	  -Dversion=3.1 
	  -Dpackaging=apklib 
	  -Durl=file:///Users/Peter/github/public-mvn-repo/releases/
	</code></pre>
	
	You will also need to release parent pom of the apklib in order for project to resolve dependencies

3. push the changes


