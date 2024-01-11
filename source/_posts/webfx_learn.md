---
title: Notes for learning to use WebFX on Windows
date: 2023-12-16 20:00:00
---

### Notes for learning to use WebFX on Windows

WebFX can be an alternative choice to transpile a JavaFX application into a web based app which can run on multi platforms.

Here is the [documentation of WebFX](https://docs.webfx.dev/#_what_is_webfx).

#### Prerequisites

A local installation of Maven is required, will be used to install the WebFX CLI.

Here is a [recommended tutorial](https://learnku.com/articles/67402).

Note: If change the local repository path of Maven, you need to specify the setting.xml file in your IDE.

#### Installing the WebFX CLI

Clone from repository:

```shell
git clone https://github.com/webfx-project/webfx-cli.git
```

Build with Maven:

```shell
cd webfx-cli #the folder you cloned to
mvn package
```

Add the webfx-cli repository (path to *webfx-cli* folder) to environment path.

Now enter following command in terminal:

```shell
webfx --help
```

Install successful once return following content:

```shell
Usage: webfx [-hV] [-D=<projectDirectory>] [--log=<logLevel>] [-M=<moduleName>]
             [COMMAND]
 __        __   _     _______  __
 \ \      / ___| |__ |  ___\ \/ /
  \ \ /\ / / _ | '_ \| |_   \  /
   \ V  V |  __| |_) |  _|  /  \
    \_/\_/ \___|_.__/|_|   /_/\_\

  -D, --directory=<projectDirectory>
                         Directory of the webfx.xml project.
  -h, --help             Show this help message and exit.
      --log=<logLevel>   Change the log level.
  -M, --module=<moduleName>
                         Name of the working module.
  -V, --version          Print version information and exit.
Commands:
  init     Initialize a WebFX repository.
  create   Create WebFX module(s).
  build    Build a WebFX application.
  run      Run a WebFX application.
  update   Update the build chain from webfx.xml files.
  rename   Rename module(s) - (experimental).
  bump     Bump to a new version if available.
  install  Install or upgrade a third-party software.
```



#### Creating first WebFX application

Enter following command to create:

```shell
webfx create application --prefix webfx-example org.example.webfxexample.WebFxExampleApplication --helloWorld
```

`webfx-example` >> module name

`org.example.webfxexample.WebFxExampleApplication` >> fully qualified name of the JavaFX class

`--helloWorld` >> a "Hello-World" template



#### Building the application to generate the executable files

Some builds require the installation of third-party software. Hence, we just try the simplest way here, to create html files.

##### Create `pom.xml` files for each child modules 

Every module should have a `pom.xml` file when build in Maven.

By default, no pom.xml file is created for the child module.

This step can be finished by IDE automatically.

Open the project in Intellij (or other IDE), open the `pom.xml` of `webfx-example` package, to create files for each child modules, then update the Maven finally to finish.

##### Build the application

Enter following command:

```shell
webfx build --wgt
```

However, there are some errors. A sample output shows as following:

```shell
PS E:\IntelliJ_Proj\webfx-example> webfx build --gwt
E:\IntelliJ_Proj\webfx-example> mvn package -P gwt-compile
[INFO] Scanning for projects...
[WARNING] The requested profile "gwt-compile" could not be activated because it does not exist.
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] webfx-example-application                                          [jar]
[INFO] webfx-example-application-openjfx                                  [jar]
[INFO] webfx-example-application-gwt                                      [jar]
[INFO] webfx-example-application-gluon                                    [jar]
[INFO] webfx-example                                                      [pom]
[INFO]
[INFO] ---------------< org.example:webfx-example-application >----------------
[INFO] Building webfx-example-application 1.0.0-SNAPSHOT                  [1/5]
[INFO]   from webfx-example-application\pom.xml
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- resources:3.3.1:resources (default-resources) @ webfx-example-application ---
[INFO] Copying 0 resource from src\main\resources to target\classes
[INFO]
[INFO] --- compiler:3.11.0:compile (default-compile) @ webfx-example-application ---
[INFO] Changes detected - recompiling the module! :source
[INFO] Compiling 1 source file with javac [debug target 20] to target\classes
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for webfx-example 1.0.0-SNAPSHOT:
[INFO]
[INFO] webfx-example-application .......................... FAILURE [  0.501 s]
[INFO] webfx-example-application-openjfx .................. SKIPPED
[INFO] webfx-example-application-gwt ...................... SKIPPED
[INFO] webfx-example-application-gluon .................... SKIPPED
[INFO] webfx-example ...................................... SKIPPED
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.595 s
[INFO] Finished at: 2023-12-16T21:00:54Z
[INFO] ------------------------------------------------------------------------
[WARNING] The requested profile "gwt-compile" could not be activated because it does not exist.
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:3.11.0:compile (default-compile) on project webfx-example-application: Fatal error compiling: 閿欒: 鏃犳晥鐨勭洰鏍囧彂琛岀増锛?0 -> [Help 1]m
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException
Call duration: 1816 ms
```

Error stack traces:

```shell
[INFO] Error stacktraces are turned on.
[DEBUG] Message scheme: color
[DEBUG] Message styles: debug info warning error success failure strong mojo project
[DEBUG] Reading global settings from D:\Maven\apache-maven-3.9.6\conf\settings.xml
[DEBUG] Reading user settings from C:\Users\20512\.m2\settings.xml
[DEBUG] Reading global toolchains from D:\Maven\apache-maven-3.9.6\conf\toolchains.xml
[DEBUG] Reading user toolchains from C:\Users\20512\.m2\toolchains.xml
[DEBUG] Using local repository at D:\Maven\home\repository
[DEBUG] Using manager EnhancedLocalRepositoryManager with priority 10.0 for D:\Maven\home\repository
[INFO] Scanning for projects...
[DEBUG] Extension realms for project org.example:webfx-example:pom:1.0.0-SNAPSHOT: (none)
[DEBUG] Looking up lifecycle mappings for packaging pom from ClassRealm[plexus.core, parent: null]
[DEBUG] Extension realms for project org.example:webfx-example-application:jar:1.0.0-SNAPSHOT: (none)
[DEBUG] Looking up lifecycle mappings for packaging jar from ClassRealm[plexus.core, parent: null]
[DEBUG] Extension realms for project org.example:webfx-example-application-openjfx:jar:1.0.0-SNAPSHOT: (none)
[DEBUG] Looking up lifecycle mappings for packaging jar from ClassRealm[plexus.core, parent: null]
[DEBUG] Extension realms for project org.example:webfx-example-application-gwt:jar:1.0.0-SNAPSHOT: (none)
[DEBUG] Looking up lifecycle mappings for packaging jar from ClassRealm[plexus.core, parent: null]
[DEBUG] Extension realms for project org.example:webfx-example-application-gluon:jar:1.0.0-SNAPSHOT: (none)
[DEBUG] Looking up lifecycle mappings for packaging jar from ClassRealm[plexus.core, parent: null]
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] webfx-example-application                                          [jar]
[INFO] webfx-example-application-openjfx                                  [jar]
[INFO] webfx-example-application-gwt                                      [jar]
[INFO] webfx-example-application-gluon                                    [jar]
[INFO] webfx-example                                                      [pom]
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for webfx-example 1.0.0-SNAPSHOT:
[INFO]
[INFO] webfx-example-application .......................... SKIPPED
[INFO] webfx-example-application-openjfx .................. SKIPPED
[INFO] webfx-example-application-gwt ...................... SKIPPED
[INFO] webfx-example-application-gluon .................... SKIPPED
[INFO] webfx-example ...................................... SKIPPED
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.089 s
[INFO] Finished at: 2023-12-16T19:53:27Z
[INFO] ------------------------------------------------------------------------
[ERROR] No goals have been specified for this build. You must specify a valid lifecycle phase or a goal in the format <plugin-prefix>:<goal> or <plugin-group-id>:<plugin-artifact-id>[:<plugin-version>]:<goal>. Available lifecycle phases are: pre-clean, clean, post-clean, validate, initialize, generate-sources, process-sources, generate-resources, process-resources, compile, process-classes, generate-test-sources, process-test-sources, generate-test-resources, process-test-resources, test-compile, process-test-classes, test, prepare-package, package, pre-integration-test, integration-test, post-integration-test, verify, install, deploy, pre-site, site, post-site, site-deploy. -> [Help 1]
org.apache.maven.lifecycle.NoGoalSpecifiedException: No goals have been specified for this build. You must specify a valid lifecycle phase or a goal in the format <plugin-prefix>:<goal> or <plugin-group-id>:<plugin-artifact-id>[:<plugin-version>]:<goal>. Available lifecycle phases are: pre-clean, clean, post-clean, validate, initialize, generate-sources, process-sources, generate-resources, process-resources, compile, process-classes, generate-test-sources, process-test-sources, generate-test-resources, process-test-resources, test-compile, process-test-classes, test, prepare-package, package, pre-integration-test, integration-test, post-integration-test, verify, install, deploy, pre-site, site, post-site, site-deploy.
    at org.apache.maven.lifecycle.internal.LifecycleStarter.execute (LifecycleStarter.java:91)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:261)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:173)
    at org.apache.maven.DefaultMaven.execute (DefaultMaven.java:101)
    at org.apache.maven.cli.MavenCli.execute (MavenCli.java:906)
    at org.apache.maven.cli.MavenCli.doMain (MavenCli.java:283)
    at org.apache.maven.cli.MavenCli.main (MavenCli.java:206)
    at jdk.internal.reflect.DirectMethodHandleAccessor.invoke (DirectMethodHandleAccessor.java:104)
    at java.lang.reflect.Method.invoke (Method.java:578)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced (Launcher.java:283)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launch (Launcher.java:226)
    at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode (Launcher.java:407)
    at org.codehaus.plexus.classworlds.launcher.Launcher.main (Launcher.java:348)
[ERROR]
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/NoGoalSpecifiedException
[DEBUG] Shutting down adapter factory; available factories [file-lock, rwlock-local, semaphore-local, noop]; available name mappers [discriminating, file-gav, file-hgav, file-static, gav, static]
[DEBUG] Shutting down 'file-lock' factory
[DEBUG] Shutting down 'rwlock-local' factory
[DEBUG] Shutting down 'semaphore-local' factory
[DEBUG] Shutting down 'noop' factory
```

> Currently I stuck on this step. 

Possible solutions (?) and some reference

Add dependency for gwt plugin

```java
<dependency>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>gwt-maven-plugin</artifactId>
    <version>2.10.0</version>
</dependency>
```

https://stackoverflow.com/questions/50673261/maven-compilation-error-failed-to-execute-goal-org-apache-maven-pluginsmaven

http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException

#### Reference

https://docs.webfx.dev/#_what_is_webfx

https://learnku.com/articles/67402

https://stackoverflow.com/questions/26021141/maven-child-module-does-not-exist

https://stackoverflow.com/questions/50673261/maven-compilation-error-failed-to-execute-goal-org-apache-maven-pluginsmaven

