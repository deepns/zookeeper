# Notes on building zookeeper

## macOS

Trying to build zookeeper on macOS

1. Pulled the latest version from github
2. Installed homebrew
3. Installed maven using homebrew `brew install mvn`
4. Ran `mvn clean build`

Failed in running the tests

```console
[ERROR] Failures:
[ERROR]   ClientRequestTimeoutTest.testClientRequestTimeout:66 waiting for server 2 being up ==> expected: <true> but was: <false>
[ERROR]   KerberosTicketRenewalTest.shouldRecoverIfKerberosNotAvailableForSomeTime:187->access$100:57->assertEventually:216 execution exceeded timeout of 15000 ms by 18302 ms
[ERROR]   QuorumPeerMainTest.testLeaderElectionWithDisloyalVoter_stillHasMajority:1190->testLeaderElection:1237 Server 1 should have joined quorum by now ==> expected: <true> but was: <false
>
[ERROR]   ReconfigBackupTest.testVersionOfDynamicFilename:287 waiting for server 3 being up ==> expected: <true> but was: <false>
[ERROR]   ReconfigRollingRestartCompatibilityTest.testRollingRestartWithExtendedMembershipConfig:232 waiting for server 1 being up ==> expected: <true> but was: <false>
[ERROR] Errors:
[ERROR]   SaslAuthTest.testThreadsShutdownOnAuthFailed:219 » Timeout Failed to connect t...
[ERROR]   CnxManagerTest.testCnxManagerListenerThreadConfigurableRetry:309 » Bind Addres...
[ERROR]   DisconnectedWatcherTest.testManyChildWatchersAutoReset » Timeout testManyChild...
[INFO]
[ERROR] Tests run: 2982, Failures: 5, Errors: 3, Skipped: 4
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for Apache ZooKeeper 3.9.0-SNAPSHOT:
[INFO]
[INFO] Apache ZooKeeper ................................... SUCCESS [  3.114 s]
[INFO] Apache ZooKeeper - Documentation ................... SUCCESS [  1.328 s]
[INFO] Apache ZooKeeper - Jute ............................ SUCCESS [ 11.209 s]
[INFO] Apache ZooKeeper - Server .......................... FAILURE [27:17 min]
[INFO] Apache ZooKeeper - Metrics Providers ............... SKIPPED
[INFO] Apache ZooKeeper - Prometheus.io Metrics Provider .. SKIPPED
[INFO] Apache ZooKeeper - Client .......................... SKIPPED
[INFO] Apache ZooKeeper - Recipes ......................... SKIPPED
[INFO] Apache ZooKeeper - Recipes - Election .............. SKIPPED
[INFO] Apache ZooKeeper - Recipes - Lock .................. SKIPPED
[INFO] Apache ZooKeeper - Recipes - Queue ................. SKIPPED
[INFO] Apache ZooKeeper - Assembly ........................ SKIPPED
[INFO] Apache ZooKeeper - Compatibility Tests ............. SKIPPED
[INFO] Apache ZooKeeper - Compatibility Tests - Curator ... SKIPPED
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  27:33 min
[INFO] Finished at: 2022-08-18T16:43:17-04:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-surefire-plugin:2.22.1:test (default-test) on project zookeeper: There are test failures.
[ERROR]
```

- Read from [here](https://issues.apache.org/jira/browse/ZOOKEEPER-3879?jql=project%20%3D%20ZOOKEEPER%20AND%20status%20%3D%20Open%20AND%20component%20%3D%20%22c%20client%22) unit tests can be flaky sometimes
- Retried the build with `mvn clean install -P full-build -DskipTests`. Failed again

```console
main:
    [mkdir] Created dir: /Users/deepan/workspace/github/zookeeper/zookeeper-client/zookeeper-client-c/target/c
[INFO] Executed tasks
[INFO]
[INFO] --- exec-maven-plugin:1.6.0:exec (autoreconf) @ zookeeper-client-c ---
[ERROR] Command execution failed.
java.io.IOException: Cannot run program "autoreconf" (in directory "/Users/deepan/workspace/github/zookeeper/zookeeper-client/zookeeper-client-c"): error=2, No such file or directory
    at java.lang.ProcessBuilder.start (ProcessBuilder.java:1143)
    at java.lang.ProcessBuilder.start (ProcessBuilder.java:1073)
    at java.lang.Runtime.exec (Runtime.java:615)
    at org.apache.commons.exec.launcher.Java13CommandLauncher.exec (Java13CommandLauncher.java:61)
    at org.apache.commons.exec.DefaultExecutor.launch (DefaultExecutor.java:279)
    at org.apache.commons.exec.DefaultExecutor.executeInternal (DefaultExecutor.java:336)
    at org.apache.commons.exec.DefaultExecutor.execute (DefaultExecutor.java:166)
    at org.codehaus.mojo.exec.ExecMojo.executeCommandLine (ExecMojo.java:804)
    at org.codehaus.mojo.exec.ExecMojo.executeCommandLine (ExecMojo.java:751)
    at org.codehaus.mojo.exec.ExecMojo.execute (ExecMojo.java:313)
    at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo (DefaultBuildPluginManager.java:137)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute2 (MojoExecutor.java:370)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute (MojoExecutor.java:351)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:215)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:171)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:163)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:117)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:81)
    at org.apache.maven.lifecycle.internal.builder.singlethreaded.SingleThreadedBuilder.build (SingleThreadedBuilder.java:56)
    at org.apache.maven.lifecycle.internal.LifecycleStarter.execute (LifecycleStarter.java:128)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:294)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:192)
    at org.apache.maven.DefaultMaven.execute (DefaultMaven.java:105)
    at org.apache.maven.cli.MavenCli.execute (MavenCli.java:960)
    at org.apache.maven.cli.MavenCli.doMain (MavenCli.java:293)
    at org.apache.maven.cli.MavenCli.main (MavenCli.java:196)
    at jdk.internal.reflect.DirectMethodHandleAccessor.invoke (DirectMethodHandleAccessor.java:104)
    at java.lang.reflect.Method.invoke (Method.java:577)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced (Launcher.java:282)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launch (Launcher.java:225)
    at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode (Launcher.java:406)
    at org.codehaus.plexus.classworlds.launcher.Launcher.main (Launcher.java:347)
Caused by: java.io.IOException: error=2, No such file or directory

[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for Apache ZooKeeper 3.9.0-SNAPSHOT:
[INFO]
[INFO] Apache ZooKeeper ................................... SUCCESS [  3.395 s]
[INFO] Apache ZooKeeper - Documentation ................... SUCCESS [  1.400 s]
[INFO] Apache ZooKeeper - Jute ............................ SUCCESS [ 10.766 s]
[INFO] Apache ZooKeeper - Server .......................... SUCCESS [ 25.990 s]
[INFO] Apache ZooKeeper - Metrics Providers ............... SUCCESS [  0.303 s]
[INFO] Apache ZooKeeper - Prometheus.io Metrics Provider .. SUCCESS [  2.413 s]
[INFO] Apache ZooKeeper - Client .......................... SUCCESS [  0.214 s]
[INFO] Apache ZooKeeper - Client - C ...................... FAILURE [  0.174 s]
[INFO] Apache ZooKeeper - Recipes ......................... SKIPPED
[INFO] Apache ZooKeeper - Recipes - Election .............. SKIPPED
[INFO] Apache ZooKeeper - Recipes - Lock .................. SKIPPED
[INFO] Apache ZooKeeper - Recipes - Queue ................. SKIPPED
[INFO] Apache ZooKeeper - Assembly ........................ SKIPPED
[INFO] Apache ZooKeeper - Compatibility Tests ............. SKIPPED
[INFO] Apache ZooKeeper - Compatibility Tests - Curator ... SKIPPED
[INFO] Apache ZooKeeper - Tests ........................... SKIPPED
[INFO] Apache ZooKeeper - Contrib ......................... SKIPPED
[INFO] Apache ZooKeeper - Contrib - Fatjar ................ SKIPPED
[INFO] Apache ZooKeeper - Contrib - Loggraph .............. SKIPPED
[INFO] Apache ZooKeeper - Contrib - Rest .................. SKIPPED
[INFO] Apache ZooKeeper - Contrib - ZooInspector .......... SKIPPED
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  44.900 s
[INFO] Finished at: 2022-08-19T10:59:11-04:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.codehaus.mojo:exec-maven-plugin:1.6.0:exec (autoreconf) on project zookeeper-client-c: Command execution failed.: Cannot run program "autoreconf" (in direc
tory "/Users/deepan/workspace/github/zookeeper/zookeeper-client/zookeeper-client-c"): error=2, No such file or directory -> [Help 1]
```

- Realized the autoconf and automake were not installed. Installed them with `brew install autoconf automake`.
- Then tried `mvn clean install -P full-build -DskipTests` again. Failed again.

```console
[INFO] --- exec-maven-plugin:1.6.0:exec (autoreconf) @ zookeeper-client-c ---
aclocal: warning: couldn't open directory '/usr/share/aclocal': No such file or directory
acinclude.m4:315: warning: macro 'AM_PATH_CPPUNIT' not found in library
configure.ac:38: error: Missing AM_PATH_CPPUNIT or PKG_CHECK_MODULES m4 macro.
acinclude.m4:317: CHECK_CPPUNIT is expanded from...
configure.ac:38: the top level
autom4te: error: /usr/local/opt/m4/bin/m4 failed with exit status: 1
aclocal: error: /usr/local/Cellar/autoconf/2.71/bin/autom4te failed with exit status: 1
autoreconf: error: aclocal failed with exit status: 1
[ERROR] Command execution failed.
org.apache.commons.exec.ExecuteException: Process exited with an error: 1 (Exit value: 1)
    at org.apache.commons.exec.DefaultExecutor.executeInternal (DefaultExecutor.java:404)
    at org.apache.commons.exec.DefaultExecutor.execute (DefaultExecutor.java:166)
    at org.codehaus.mojo.exec.ExecMojo.executeCommandLine (ExecMojo.java:804)
    at org.codehaus.mojo.exec.ExecMojo.executeCommandLine (ExecMojo.java:751)
    at org.codehaus.mojo.exec.ExecMojo.execute (ExecMojo.java:313)
    at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo (DefaultBuildPluginManager.java:137)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute2 (MojoExecutor.java:370)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute (MojoExecutor.java:351)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:215)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:171)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:163)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:117)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:81)
    at org.apache.maven.lifecycle.internal.builder.singlethreaded.SingleThreadedBuilder.build (SingleThreadedBuilder.java:56)
    at org.apache.maven.lifecycle.internal.LifecycleStarter.execute (LifecycleStarter.java:128)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:294)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:192)
    at org.apache.maven.DefaultMaven.execute (DefaultMaven.java:105)
    at org.apache.maven.cli.MavenCli.execute (MavenCli.java:960)
    at org.apache.maven.cli.MavenCli.doMain (MavenCli.java:293)
    at org.apache.maven.cli.MavenCli.main (MavenCli.java:196)
    at jdk.internal.reflect.DirectMethodHandleAccessor.invoke (DirectMethodHandleAccessor.java:104)
    at java.lang.reflect.Method.invoke (Method.java:577)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced (Launcher.java:282)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launch (Launcher.java:225)
    at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode (Launcher.java:406)
    at org.codehaus.plexus.classworlds.launcher.Launcher.main (Launcher.java:347)
```

- Found the same issue in the discussion threads of [here](https://issues.apache.org/jira/browse/ZOOKEEPER-3879?jql=project%20%3D%20ZOOKEEPER%20AND%20status%20%3D%20Open%20AND%20component%20%3D%20%22c%20client%22) and in [gitter talk](https://gitter.im/apache/apache-zookeeper?at=5d6e2b041e31671227fcc24a)
- Installed cppunit `brew install cppunit`. Then tried `mvn clean install -P full-build -DskipTests`. Same result.

```console
[INFO] --- exec-maven-plugin:1.6.0:exec (autoreconf) @ zookeeper-client-c ---
aclocal: warning: couldn't open directory '/usr/share/aclocal': No such file or directory
acinclude.m4:315: warning: macro 'AM_PATH_CPPUNIT' not found in library
configure.ac:38: error: Missing AM_PATH_CPPUNIT or PKG_CHECK_MODULES m4 macro.
acinclude.m4:317: CHECK_CPPUNIT is expanded from...
configure.ac:38: the top level
autom4te: error: /usr/local/opt/m4/bin/m4 failed with exit status: 1
aclocal: error: /usr/local/Cellar/autoconf/2.71/bin/autom4te failed with exit status: 1
autoreconf: error: aclocal failed with exit status: 1
[ERROR] Command execution failed.
org.apache.commons.exec.ExecuteException: Process exited with an error: 1 (Exit value: 1)
    at org.apache.commons.exec.DefaultExecutor.executeInternal (DefaultExecutor.java:404)
    at org.apache.commons.exec.DefaultExecutor.execute (DefaultExecutor.java:166)
    at org.codehaus.mojo.exec.ExecMojo.executeCommandLine (ExecMojo.java:804)
    at org.codehaus.mojo.exec.ExecMojo.executeCommandLine (ExecMojo.java:751)
    at org.codehaus.mojo.exec.ExecMojo.execute (ExecMojo.java:313)
    at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo (DefaultBuildPluginManager.java:137)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute2 (MojoExecutor.java:370)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute (MojoExecutor.java:351)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:215)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:171)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:163)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:117)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:81)
    at org.apache.maven.lifecycle.internal.builder.singlethreaded.SingleThreadedBuilder.build (SingleThreadedBuilder.java:56)
    at org.apache.maven.lifecycle.internal.LifecycleStarter.execute (LifecycleStarter.java:128)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:294)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:192)
    at org.apache.maven.DefaultMaven.execute (DefaultMaven.java:105)
    at org.apache.maven.cli.MavenCli.execute (MavenCli.java:960)
    at org.apache.maven.cli.MavenCli.doMain (MavenCli.java:293)
    at org.apache.maven.cli.MavenCli.main (MavenCli.java:196)
    at jdk.internal.reflect.DirectMethodHandleAccessor.invoke (DirectMethodHandleAccessor.java:104)
    at java.lang.reflect.Method.invoke (Method.java:577)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced (Launcher.java:282)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launch (Launcher.java:225)
    at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode (Launcher.java:406)
    at org.codehaus.plexus.classworlds.launcher.Launcher.main (Launcher.java:347)
[INFO] --
```

## Debian

- Tried it on GCP Cloud Shell VM since it comes with all prerequisite packages
- Pulled zookeeper repo
- run the script `dev/docker/run.sh`, didn't realize that it didn't include the install command.

```console
+ pushd /home/deepan_seeralan/github/zookeeper/dev/docker/../..
~/github/zookeeper ~/github/zookeeper
++ realpath /home/deepan_seeralan/github/zookeeper/dev/docker/../..
+ docker run -i -t --rm=true -w /home/deepan_seeralan/github/zookeeper/dev/docker/../.. -u deepan_seeralan -v /home/deepan_seeralan/github/zookeeper:/home/deepan_seeralan/github/zookeeper/dev/docker/../.. -v /home/deepan_seeralan:/home/deepan_seeralan zookeeper/dev-deepan_seer
alan bash -c '
echo
echo '\''Welcome to Apache ZooKeeper Development Env'\''
echo '\''To build, execute'\''
echo '\''  mvn clean install'\''
echo
bash
'
mkdir: cannot create directory ‘/root’: Permission denied
Can not write to /root/.m2/copy_reference_file.log. Wrong volume permissions? Carrying on ...

Welcome to Apache ZooKeeper Development Env
To build, execute
  mvn clean install

bash: /google/devshell/bashrc.google: No such file or directory
```

- but could see the container was built.

```console
deepan_seeralan@cloudshell:~$ docker images
REPOSITORY                      TAG           IMAGE ID       CREATED          SIZE
zookeeper/dev-deepan_seeralan   latest        3727bc4f1935   46 minutes ago   847MB
zookeeper/dev                   latest        8e0f1ca74c32   46 minutes ago   847MB
maven                           3.6.3-jdk-8   d1b3f61d61f2   16 months ago    525MB
deepan_seeralan@cloudshell:~$
```

- then ran this command manually. ` docker run --rm -it -w /root/zk -v "$PWD:/root/zk" zookeeper/dev mvn clean install -Pfull-build -DskipTests`. That built successfully

```console
[INFO] Reactor Summary for Apache ZooKeeper 3.9.0-SNAPSHOT:
[INFO]
[INFO] Apache ZooKeeper ................................... SUCCESS [ 25.033 s]
[INFO] Apache ZooKeeper - Documentation ................... SUCCESS [  3.064 s]
[INFO] Apache ZooKeeper - Jute ............................ SUCCESS [ 19.537 s]
[INFO] Apache ZooKeeper - Server .......................... SUCCESS [ 31.069 s]
[INFO] Apache ZooKeeper - Metrics Providers ............... SUCCESS [  0.234 s]
[INFO] Apache ZooKeeper - Prometheus.io Metrics Provider .. SUCCESS [  2.029 s]
[INFO] Apache ZooKeeper - Client .......................... SUCCESS [  0.255 s]
[INFO] Apache ZooKeeper - Client - C ...................... SUCCESS [ 29.712 s]
[INFO] Apache ZooKeeper - Recipes ......................... SUCCESS [  0.285 s]
[INFO] Apache ZooKeeper - Recipes - Election .............. SUCCESS [  0.693 s]
[INFO] Apache ZooKeeper - Recipes - Lock .................. SUCCESS [  0.493 s]
[INFO] Apache ZooKeeper - Recipes - Queue ................. SUCCESS [  0.531 s]
[INFO] Apache ZooKeeper - Assembly ........................ SUCCESS [ 17.782 s]
[INFO] Apache ZooKeeper - Compatibility Tests ............. SUCCESS [  0.365 s]
[INFO] Apache ZooKeeper - Compatibility Tests - Curator ... SUCCESS [  1.652 s]
[INFO] Apache ZooKeeper - Tests ........................... SUCCESS [  3.659 s]
[INFO] Apache ZooKeeper - Contrib ......................... SUCCESS [  0.228 s]
[INFO] Apache ZooKeeper - Contrib - Fatjar ................ SUCCESS [  5.548 s]
[INFO] Apache ZooKeeper - Contrib - Loggraph .............. SUCCESS [  2.997 s]
[INFO] Apache ZooKeeper - Contrib - Rest .................. SUCCESS [  3.586 s]
[INFO] Apache ZooKeeper - Contrib - ZooInspector .......... SUCCESS [  8.550 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
```

- able to make changes to the c-client and test it out with cli_mt tool.