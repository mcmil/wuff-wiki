We already [created first Equinox app](Create-first-Equinox-app). Now we configure Equinox products.

### Add product definition

Edit "build.gradle", insert code:

```groovy
products {
  product platform: 'linux', arch: 'x86_32'
  product platform: 'linux', arch: 'x86_64'
  product platform: 'windows', arch: 'x86_32'
  product platform: 'windows', arch: 'x86_64'
  product platform: 'macosx', arch: 'x86_64'
  archiveProducts = true
}
```

Here we define 5 products: 32-bit and 64-bit versions for Linux, 32-bit and 64-bit versions for Windows, and 64-bit version for Mac OS X.
Optional archiveProducts flag instructs wuff to archive the generated products. Both Linux and Mac OS X versions will be 
archived as .tar.gz, Windows versions - as .zip. The default value of archiveProducts is false.

### Compile

Invoke on command line: `gradle build`

Check: there must be 4 products in "tutorials/MyEquinoxApp/build/output" folder. 

Check: Each product must contain "MyEquinoxApp" bundle in "plugins" subfolder and in "configuration/config.ini". 

### Run

We can run each product on corresponding OS and architecture. On Windows platform we start application via .bat-file, on other platforms we start application via .sh-file.

Attention: do not try to run the generated product on a "wrong" OS or "wrong" architecture. 
Linux product won't start on windows. 64-bit product won't start on 32-bit JRE.

---

The example code for this page: [examples/EquinoxApp-2](../tree/master/examples/EquinoxApp-2).

Next page: [Prepare Equinox app for multiproject build](Prepare-Equinox-app-for-multiproject-build).

