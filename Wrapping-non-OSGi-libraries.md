Wuff supports two ways to use non-OSGi libraries in OSGi/Eclipse applications: 

- wrap non-OSGi library as self-contained OSGi bundle
- include non-OSGi library into OSGi-bundle

Below are detailed how-to explanations.

## wrap non-OSGi library as self-contained OSGi bundle

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

when we build the program with `gradle build`, it creates Eclipse-RCP product in "build/output" folder. Lets have a look at "plugins" subfolder of the created product, it contains the file "jdom2-bundle-2.0.5.jar". This file contains the original "jdom2-2.0.5.jar" plus generated OSGi-manifest with appropriate package imports/exports. The file "jdom2-bundle-2.0.5.jar" is also listed in "configuration/config.ini", that means - it is available to other OSGi bundles within app.