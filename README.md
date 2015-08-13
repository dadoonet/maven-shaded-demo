Demo project for maven issue with shading
=====================


The dependency tree changes with shaded artifacts depending from where you run maven (root project or within module).

## Demo 

If you run this project with:

```sh
mvn dependency:tree
# Or
mvn dependency:tree -pl :myproject,:mylib-shaded
```

You'll get as an output for myproject module:

```
[INFO] ------------------------------------------------------------------------
[INFO] Building myproject 1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ myproject ---
[INFO] fr.pilato.maven.test:myproject:jar:1.0-SNAPSHOT
[INFO] \- fr.pilato.maven.test:mylib-shaded:jar:1.0-SNAPSHOT:compile
[INFO]    \- fr.pilato.maven.test:mylib:jar:1.0-SNAPSHOT:compile
[INFO] ------------------------------------------------------------------------
```

But if you do that within `myproject` module, then you get a correct tree:

```sh
mvn dependency:tree -f myproject/pom.xml
# Or
mvn dependency:tree -pl :myproject
```

```
[INFO] ------------------------------------------------------------------------
[INFO] Building myproject 1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ myproject ---
[INFO] fr.pilato.maven.test:myproject:jar:1.0-SNAPSHOT
[INFO] \- fr.pilato.maven.test:mylib-shaded:jar:1.0-SNAPSHOT:compile
```


We can obviously see that depending on which modules are added to the reactor, then Maven creates another 
dependency tree.

Test ran with:

```
$ mvn -version
Apache Maven 3.3.3 (7994120775791599e205a5524ec3e0dfe41d4a06; 2015-04-22T13:57:37+02:00)
Maven home: /usr/local/Cellar/maven/3.3.3/libexec
Java version: 1.7.0_60, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.7.0_60.jdk/Contents/Home/jre
Default locale: fr_FR, platform encoding: UTF-8
OS name: "mac os x", version: "10.10.4", arch: "x86_64", family: "mac"
```


Related to https://github.com/elastic/elasticsearch/pull/12803


