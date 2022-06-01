# Setting JAVA_HOME environment variable

Today I faced very weird error `unable to find valid certification path to requested target` while I was compiling my work java project.

```
[ERROR] (o.a.a.r.c.BaseReplicationProducer:97) – Error occurred while performing folder replication for 'XXXX': sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target: sun.security.validator.ValidatorException:
```

This article [ARTIFACTORY: How to Resolve an “unable to find valid certification path to requested target” Error](https://jfrog.com/knowledge-base/how-to-resolve-unable-to-find-valid-certification-path-to-requested-target-error/) helped me to narrow down the problem.

> Trust is handled by having the root and intermediate certificates of your SSL certificate on a trusted keystore. With that said, this may not be required if you're using the default JVM security setting.

If we are using default JVM security, this is not needed. This indicated to me that I am not using the default.

I ran `which java` and `which javac` and both pointed to me `/usr/bin/java` and `/usr/bin`. I checked the environment variable $JAVA_HOME which pointed that I am using java from sdkman. sdkman is similar to conda for python in javaworld. It allows to manage different java versions. So I was experimenting with sdkman to use jshell. 

So the first thing I did is to `mv ~/.sdkman ~/.sdkman.bak` and `source ~/.zshrc` to reload the zsh.

Then I ran `sudo alternatives --config java` and choose the default JVM that is part of the system. Repeat the same for `javac`. I also have $JAVA_HOME set up with `export JAVA_HOME=$(dirname $(dirname $(readlink -f $(which javac)))` as part of `/etc/environment` based on [this stackoverflow](https://askubuntu.com/questions/175514/how-to-set-java-home-for-java) post.

Now I checked the versions `$ java -version` and `$ javac -version` to ensure we are using default JVM settings and resoved the error.

   
