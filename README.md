Jetty Gradle Hello World, TDD Style
=====

I want to make a simple web app with Jetty, using Gradle as the build
tool.  But I'm big on Test Driven Development (TDD), so I want to do it
TDD style.

My first task is to get a simple Gradle build file in place.  Looking at
http://stackoverflow.com/questions/7864521/gradle-jettyrun-how-does-this-thing-work
, we get a good starter for a build.gradle file.  We add boiler-plate
java build/test stuff, and our build.gradle looks like:

```groovy
apply plugin: 'java'
apply plugin: 'jetty'

repositories {
    mavenCentral()
}
dependencies {
    testCompile 'junit:junit:4.11'
    testCompile 'org.hamcrest:hamcrest-all:1.3'

}
test {
    exclude '**/*IntegrationTest*'
}

task integrationTest(type: Test) {
    include '**/*IntegrationTest*'
    doFirst {
        jettyRun.httpPort = 8080    // Port for test
        jettyRun.daemon = true
        jettyRun.execute()
    }
    doLast {
        jettyStop.stopPort = 8091   // Port for stop signal
        jettyStop.stopKey = 'stopKey'
        jettyStop.execute()
    }
}
```

We run `gradle build` and get success.  Ready for our first test.
