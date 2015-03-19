# How to Use #

## Apache Maven ##
To make use of this library in a Maven project, add the following to your project's POM:
```
<project>
  ...
  <dependencies>
    <dependency>
      <groupId>com.google.code.ksoap2-android</groupId>
      <artifactId>ksoap2-android</artifactId>
      <version>3.4.0</version>
    </dependency>
  </dependencies>
  ...
</project>
```

and configure a proxy repository in your repository manager using the url

```
http://ksoap2-android.googlecode.com/svn/m2-repo
```

If you are not using a repository manager like [Sonatype Nexus](http://www.sonatype.org/nexus), you should start doing so now.

As a stop gap solution you could add the repository to a profile in your settings.xml or pom.xml:

```
...
<repositories>		
  <repository>		
    <id>googlecode-ksoap2-android</id>		
    <name>googlecode-ksoap2-android</name>		
    <url>http://ksoap2-android.googlecode.com/svn/m2-repo</url>		
  </repository>		
</repositories>
```

Contact [simpligility technologies](http://www.simpligility.com/contact/) for consulting and training for [Sonatype Nexus](http://www.sonatype.org/nexus).

Further libraries as part of the project are also published as part of the build and can be declared as dependencies as well, if needed.

## Standard Android Ant-based build and Eclipse/ADT ##

To make use of this library in a non-Maven project, follow the instructions in the [Android Developer's Guide](http://developer.android.com/guide/index.html) on how to [Add an External Library](http://developer.android.com/guide/appendix/faq/commontasks.html#addexternallibrary) to your project.

You will need to add a ksoap2-android and all required transitive dependencies to the build path. Luckily the Maven build of the project produces a nice bundle of all these jars in one big file.

The different version of these files are available at  http://code.google.com/p/ksoap2-android/source/browse/#svn/m2-repo/com/google/code/ksoap2-android/ksoap2-android-assembly/

**To download a file from there, right click on "View raw file" and select "Save Link as" (this label differs for different browsers) and you will get the full jar downloaded.**

The latest release artifact would be available at

http://code.google.com/p/ksoap2-android/source/browse/m2-repo/com/google/code/ksoap2-android/ksoap2-android-assembly/3.4.0/ksoap2-android-assembly-3.4.0-jar-with-dependencies.jar

with a direct download url of

http://ksoap2-android.googlecode.com/svn/m2-repo/com/google/code/ksoap2-android/ksoap2-android-assembly/3.4.0/ksoap2-android-assembly-3.4.0-jar-with-dependencies.jar

**Some users seem to experience download problems with IE. Just try a decent browser or download with a command line tool like wget**

```
wget http://ksoap2-android.googlecode.com/svn/m2-repo/com/google/code/ksoap2-android/ksoap2-android-assembly/3.4.0/ksoap2-android-assembly-3.4.0-jar-with-dependencies.jar
```

**After the manual download confirm the size of the downloaded file since many users seem to be experiencing problems.**

Or better just use Maven and a repository server.

## More ##
And in terms of how to write your code please look at the [help wiki page](http://code.google.com/p/ksoap2-android/wiki/NeedHelp).