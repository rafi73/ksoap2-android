# Source Repository: github #

The source code for this project is hosted on [github](http://github.com/) here:

Current:
http://github.com/mosabua/ksoap2-android/tree/

You can either download directly there or install git and clone the repo to your local machine.

Find more information on using github, git and associated tools on the github help section and linked content.

# Deployment Repository: Google Code #

The ource code is NOT hosted at Google Code, however this project does make use of the SVN repository for Maven deployments.  If necessary, you can explore the SVN repository via:
  * Browse: http://code.google.com/p/ksoap2-android/source/browse/
  * Changes: http://code.google.com/p/ksoap2-android/source/list

This repository contains the dependencies themselves as well as javadoc and source jar files. There is also a bundle that has all libraries together in one large file:
http://code.google.com/p/ksoap2-android/source/browse/#svn/m2-repo/com/google/code/ksoap2-android/ksoap2-android-assembly


## Command-Line Access ##

If you plan to make changes directly to the Maven repository, use this command to check it out from SVN as yourself using HTTPS:
```
# Project members authenticate over HTTPS to allow committing changes.
svn checkout https://ksoap2-android.googlecode.com/svn/m2-repo/ ksoap2-android-repo --username mygoogleusername
```

When prompted, enter your [generated googlecode.com password](http://code.google.com/hosting/settings).

Use this command to anonymously check out the repository:
```
# Non-members may check out a read-only working copy anonymously over HTTP.
svn checkout http://ksoap2-android.googlecode.com/svn/m2-repo/ ksoap2-android-repo-read-only 
```