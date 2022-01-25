# Test Resource Directory

Similar to the resource directory in the main app code, this **test/res** directory stores resources for testing.

Specify the **test/res** as a "source" directory in the build file so we can access the testing resource files (relatively) without having to type the full absolute paths.

`app/build.gradle`

```groovy
android {
    ...
    sourceSets {
       test.resources.srcDirs += 'src/test/res'
    }
}
```