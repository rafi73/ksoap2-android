# Requirement #

[Apache Maven](http://maven.apache.org/) 3.x installed.

# How to build the project and run the test #

To run the complete build including tests as well as bundle creation and so on run the command

> `mvn clean install`

Note: if your build fails make sure to check out the error message. It might be because you are violating the code style enforced by the checkstyle plugin. If that is the case check out the checkstyle-result.xml file in the target folder of the failing module.

# Coding Style #

The Maven build enforces a code style with the Maven Checkstyle Plugin to make the code more readable and consistent as well as to avoid problems with merges we had in the past when it comes to tabs vs. spaces.

The checkstyle rules are all defined https://github.com/mosabua/ksoap2-android/blob/master/build-tools/src/main/resources/simpligility/checkstyle.xml

# How to do a release build #

To do a release build with a branch created in http://github.com/mosabua/ksoap2-android use

> `mvn release:prepare`

To build the artifacts of the release and deploy them to the google code svn repo use

> `mvn release:perform`

Note however that you can only cut a release if you have access rights to the github repository and the svn repo (= you are the project owner..).

## Fingerprint problem during release perform ##

If you run into an error message like this

[INFO](INFO.md) Error validating server certificate for 'https://ksoap2-android.googlecode.com:443':
[INFO](INFO.md)  - The certificate is not issued by a trusted authority. Use the
[INFO](INFO.md)    fingerprint to validate the certificate manually!
[INFO](INFO.md)  - The certificate hostname does not match.
[INFO](INFO.md) Certificate information:
[INFO](INFO.md)  - Subject: CN=**.googlecode.com, O=Google Inc, L=Mountain View, ST=California, C=US
[INFO](INFO.md)  - Valid: from Wed Jan 05 14:46:46 PST 2011 until Thu Jan 05 14:56:46 PST 2012
[INFO](INFO.md)  - Issuer: CN=Google Internet Authority, O=Google Inc, C=US
[INFO](INFO.md)  - Fingerprint: b1:af:83:76:f3:81:b0:57:70:d8:07:42:c8:c1:b3:67:38:c8:7a:bc**



abort the release perform and run
> `mvn deploy`

> manually. This will allow you to manually accept the certificate. Afterwards you can resume the release build with

> `mvn release:perform -Dresume=true`

See http://tech--help.blogspot.com/2011/01/solved-error-validating-server.html