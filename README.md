# ETF build and deployment plugin helper

[![Latest version](http://img.shields.io/badge/latest%20version-1.0.21-blue.svg)](https://services.interactive-instruments.de/etfdev-af/plugins-releases-local/de/interactive_instruments/bda/etf-bda/1.0.21/etf-bda-1.0.21.jar)

This gradle helper script is used in ETF projects to preconfigure Gradle based Java projects.

The following block must be inserted at the beginning of a build.gradle file:

```groovy
///////////////////////////////////////////////////////////////////////////////////////

buildscript {
	repositories {
		maven {
			url "http://services.interactive-instruments.de/etfdev-af/plugins-releases-local"
			credentials {
				// Our repo does not allow empty credentials
				username 'ii-bda'
				password '6ozhS683'
			}}
	}
	dependencies {
		classpath group: 'de.interactive_instruments.bda', name: 'etf-bda', version:'1.0.19'
	}
	dependencies {
		ant.unjar src: configurations.classpath.files.find {it.path.contains('etf')}, dest: 'build/gradle'
	}
}
apply from: 'build/gradle/ii-bda.gradle'

///////////////////////////////////////////////////////////////////////////////////////
```

## What it does/configures
- Sets values in the manifest file, like the build version, current GIT revision, build date, etc.

- Configures the ii maven repositories for getting dependencies or releasing an artifact
(if the project version is suffixed with 'SNAPSHOT' -> snapshot repos will be used)

- Configures checks and the formatting of the source code

## Building projects
Build the project like every other gradle project with:
```gradle
$ gradlew build
```

The build will fail if format violations are found:
```
FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':spotlessJavaCheck'.
> Format violations were found. Run 'gradlew spotlessApply' to fix them.
	src/main/java/de/whatever.java
```

These can be formatted by running:
```gradle
$ gradlew spotlessApply
```

## Release artifacts
Add your credentials for releasing an artifact to the ii repo in the ~/.gradle/gradle.properties file:

```
ii.etfdev.repo.username=user@foo.bar
ii.etfdev.repo.password=password
```

An artifact can be released by running:
```gradle
$ gradlew release
```

## Used plugins
The following plugins are used:

- [Release plugin for releasing artifacts](https://github.com/researchgate/gradle-release)
- [Spotless plugin for checking and applying formatting](https://github.com/diffplug/spotless)
