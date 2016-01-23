# ETF build and deployment plugin helper

This plugin is used in etf for preconfiguration of gradle based Java projects.

The following block must be inserted at the beginning of the ETF build.gradle file:

```groovy
///////////////////////////////////////////////////////////////////////////////////////
buildscript {
	repositories {
		maven {
			url "http://services.interactive-instruments.de/etfdev-af/plugins-releases-local"
			credentials {
				username 'ii-bda'
				password '6ozhS683'
			}}
		maven {
			url "https://plugins.gradle.org/m2/"
		}
		jcenter()
	}
	dependencies {
		classpath group: 'de.interactive_instruments.bda', name: 'etf-bda', version:'1.0.+'
		classpath 'net.researchgate:gradle-release:2.3.4'
		classpath "gradle.plugin.nl.javadude.gradle.plugins:license-gradle-plugin:0.12.1"
	}
	dependencies {
		ant.unjar src: configurations.classpath.files.find {it.path.contains('etf')}, dest: 'build/gradle'
	}
}
apply from: 'build/gradle/ii-bda.gradle'
///////////////////////////////////////////////////////////////////////////////////////
```
