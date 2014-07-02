Wuff supports two ways to use non-OSGi libraries in OSGi/Eclipse applications: 

- Wrap non-OSGi library as self-contained OSGi bundle
- Include non-OSGi library into OSGi-bundle

Below are detailed how-to explanations.

## Wrap non-OSGi library as self-contained OSGi bundle

Whenever we add non-OSGi library as compile or runtime dependency to the OSGi app (where one of  "org.akhikhl.wuff.eclipse-equinox-app", "org.akhikhl.wuff.eclipse-ide-app" or "org.akhikhl.wuff.eclipse-rcp-app" is applied), it is automatically wrapped as OSGi bundle. Example:

```groovy
buildscript {
  repositories {
    mavenLocal()
    jcenter()
  }

  dependencies {
    classpath 'org.akhikhl.wuff:wuff-plugin:+'
  }
}

apply plugin: 'java'
apply plugin: 'org.akhikhl.wuff.eclipse-rcp-app'

repositories {
  mavenLocal()
  jcenter()
}

dependencies {
  compile 'org.jdom:jdom2:2.0.5'
}
```

When we invoke `gradle build`, Wuff creates Eclipse-RCP product in "build/output" directory. Let's have a look at "plugins" subfolder of the created product, it contains the file "jdom2-bundle-2.0.5.jar". This file contains the original "jdom2-2.0.5.jar" plus generated OSGi-manifest with appropriate package imports/exports. The file "jdom2-bundle-2.0.5.jar" is also listed in "configuration/config.ini", that means - it is available to other OSGi bundles within app.

Note that wrapping non-OSGi libraries to OSGi bundles is automatically applied to all transitive dependencies.

## Include non-OSGi library into OSGi-bundle

There are cases when wrapping non-OSGi library as self-contained OSGi-bundle is not desirable. For example, we treat non-OSGi library is an implementation detail of some other bundle and don't want other parts of the program to see that library. Or we might want to use some specific version of non-OSGi library for a particular bundle, while globally we use another version.

Specifically for such cases Wuff supports inclusion of non-OSGi libraries into OSGi bundles. Example:

```groovy
buildscript {
  repositories {
    mavenLocal()
    jcenter()
  }

  dependencies {
    classpath 'org.akhikhl.wuff:wuff-plugin:+'
  }
}

repositories {
  mavenLocal()
  jcenter()
}

apply plugin: 'java'
apply plugin: 'org.akhikhl.wuff.eclipse-bundle'


dependencies {
  privateLib 'org.jdom:jdom2:2.0.5'
}
```

Please note that we use "privateLib" configuration, not "compile" or "runtime". This is special configuration for including non-OSGi libraries into OSGi bundles.

When we invoke `gradle build`, Wuff creates an OSGi bundle in "build/libs" directory. Let's have a look inside the generated jar-file: it now contains "jdom2-2.0.5.jar". The generated OSGi-manifest contains instruction `Bundle-ClassPath: .,jdom2-2.0.5.jar`, which makes non-OSGi library accessible to OSGi-bundle at runtime. Note that OSGi-bundle does not export any packages provided by non-OSGi library, that means - library is an implementation detail of OSGi bundle and it is not accessible in other bundles.