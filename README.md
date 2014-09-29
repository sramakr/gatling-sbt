# gatling-sbt   [![Build Status](https://travis-ci.org/gatling/gatling-sbt.svg?branch=master)](https://travis-ci.org/gatling/gatling-sbt)


This SBT plugin integrates Gatling with SBT, allowing to use Gatling as a testing framework.

## Setup 

Snapshots are available on Sonatype.

In `project/plugins.sbt`, add: 

    addSbtPlugin("io.gatling" % "sbt-plugin" % "1.0-RC5")
    
You'll also need those two dependencies (use `"it"` scope for the `GatlingIt` configuration):

```scala
"io.gatling.highcharts" % "gatling-charts-highcharts" % "2.0.0-RC5" % "test"
"io.gatling" % "test-framework" % "1.0-RC5" % "test"
```

And then, in your `.scala` build :

```scala

import io.gatling.sbt.GatlingPlugin

lazy val project = Project(...)
                     .enablePlugins(GatlingPlugin)
				     .settings(libraryDependencies ++= /* Gatling dependencies */)

```

or in your `.sbt` file :

```scala

val test = project.in(file("."))
  .enablePlugins(GatlingPlugin)
  .settings(libraryDependencies ++= /* Gatling dependencies */)

```
## Usage 

As with any SBT testing framework, you'll be able to run Gatling simulations using SBT standard `test`, `testOnly`, `testQuick`, etc... tasks. Note however that they must be prefixed by `gatling:` (or `gatling-it` if you're using the `GatlingIt` configuration), eg. `gatling:test`, `gatling:testOnly`, `gatling:testQuick`, etc...

## Default settings 

For the `Gatling` configuration :

* By default, Gatling simulations must be in `src/test/scala`, configurable using the `scalaSource in Gatling` setting.
* By default, Gatling reports are written to `target/gatling`, configurable using the `target in Gatling` setting.

For the `GatlingIt` configuration :

* By default, Gatling simulations must be in `src/it/scala`, configurable using the `scalaSource in GatlingIt` setting.
* By default, Gatling reports are written to `target/gatling-it`, configurable using the `target in GatlingIt` setting.

## Additional tasks

Gatling's SBT plugin also offers three additional tasks:

* `startRecorder`, which start the Recorder, configured to save recorded simulations to the location specified by `scalaSource in Gatling` (by default, `src/test/scala`).
* `lastReport`, which opens the last generated report in your web browser.
* `copyConfigFiles`, which copies Gatling's configuration files (gatling.conf & recorder.conf) from the bundle into your project resources if they're missing. Please not that you need to add Gatling's bundle to your dependencies for this task to work : `"io.gatling" % "gatling-bundle" % "2.0.0-RC5" % "test" artifacts (Artifact("gatling-bundle", "zip", "zip", "bundle"))`. After the configuration files have been copied, this dependency can be safely dropped.
