Provided that we already [configured RCP products](Configure-RCP-products), we can now prepare RCP app for multiproject build.

1. Create "build.gradle" in "tutorials" folder (parent of "MyRcpApp" folder) 

2. Move "buildscript" and "repositories" from "tutorials/MyRcpApp/build.gradle" to "tutorials/build.gradle", so that two scripts look like this:

  "tutorials/build.gradle":
  ```groovy
  buildscript {
    repositories {
      mavenLocal()
      jcenter()
    }
    
    dependencies {
      classpath 'org.akhikhl.wuff:wuff-plugin:0.0.1'
    }
  }

  subprojects {
    repositories {
      mavenLocal()
      jcenter()
    }
  }
  ```

  "tutorials/MyRcpApp/build.gradle":
  ```groovy
  apply plugin: 'java'
  apply plugin: 'eclipse-rcp-app'
    
  products {
    product platform: 'linux', arch: 'x86_32'
    product platform: 'linux', arch: 'x86_64'
    product platform: 'windows', arch: 'x86_32'
    product platform: 'windows', arch: 'x86_64'
    archiveProducts = true
  }
  ```

3. Create "settings.gradle" in "tutorials" folder (parent of "MyRcpApp" folder), insert code:

  ```groovy
  include 'MyRcpApp'
  ```

4. Invoke on command line in "tutorials" folder:

  ```shell
  gradle build
  ```

  **CHECK**: Build task generates products in "tutorials/MyRcpApp/build/output" folder.

The example code corresponding to this page is located in [tutorialExamples/RcpApp-3](../tree/master/tutorialExamples/RcpApp-3).

Now we can move on to [Create Eclipse bundle and use it in RCP app](Create-Eclipse-bundle-and-use-it-in-RCP-app).