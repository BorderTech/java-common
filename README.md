# java-common

Reusable build configuration for BorderTech open source projects.

## Status
[![Build Status](https://travis-ci.com/BorderTech/java-common.svg?branch=master)](https://travis-ci.com/BorderTech/java-common)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=bordertech-java-common&metric=alert_status)](https://sonarcloud.io/dashboard?id=bordertech-java-common)
[![Reliability Rating](https://sonarcloud.io/api/project_badges/measure?project=bordertech-java-common&metric=reliability_rating)](https://sonarcloud.io/dashboard?id=bordertech-java-common)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/c7a2226acd574943af9ae966c54b05e6)](https://app.codacy.com/app/BorderTech/java-common?utm_source=github.com&utm_medium=referral&utm_content=BorderTech/java-common&utm_campaign=Badge_Grade_Dashboard)
[![Maven Central](https://img.shields.io/maven-central/v/com.github.bordertech.common/bordertech-parent.svg?label=Maven%20Central)](https://search.maven.org/search?q=g:%22com.github.bordertech.common%22%20AND%20a:%22bordertech-parent%22)

## qa-parent

BorderTech java projects should generally use this as their parent POM.

``` xml
<project>
	....
	<parent>
		<groupId>com.github.bordertech.common</groupId>
		<artifactId>qa-parent</artifactId>
		<version>1.0.14</version>
	</parent>
	....
</project>
```

It runs quality assurance checks on your java code using tools such as checkstyle, pmd, spotbugs.

By default qa checks do not run, you must enable them on a per-module basis or parent pom like so:

``` xml
<properties>
	<!-- Set bt.qa.skip to false to run QA checks. -->
	<bt.qa.skip>false</bt.qa.skip>
</properties>
```
Refer to qa-parent's [pom.xml](https://github.com/BorderTech/java-common/blob/master/qa-parent/pom.xml) for other project properties.

The qa-parent also runs:
* [OWASP plugin](https://jeremylong.github.io/DependencyCheck/dependency-check-maven/index.html) to check security vulnerabilities
* [Enforcer plugin](https://maven.apache.org/enforcer/maven-enforcer-plugin/) to check dependency convergence
* [Version checker plugin](https://www.mojohaus.org/versions-maven-plugin/) to report project dependencies that have new versions available

The qa-parent inherits all of the release functionality from bordertech-parent, discussed below.

## bordertech-parent

This is the top-level pom.xml file.
It configures the maven release plugin for open source BorderTech projects to release to Maven Central.

_Note that java projects should generally not consume this directly but instead should use qa-parent as a parent POM instead._

Projects using this must ensure the necessary POM sections are overriden - these are marked in the bordertech-parent pom, for example:

``` xml
<!--
  Descendants SHOULD override the url.
-->
<url>https://github.com/bordertech/java-common/</url>
```

Once you have configured your project and environment you can release to Maven Central. It may look a little something like the examples below.

### Releasing

The golden rule is ALWAYS do the release on a separate branch (it makes [backing out](https://github.com/BorderTech/java-common/wiki/Releasing#dealing-with-failure) much easier when problems arise).

Full documentation is available in the wiki under [Releasing](https://github.com/BorderTech/java-common/wiki/Releasing).

## build-tools

This is primarily a shared resources module used by qa-parent and potentially other BorderTech maven modules.

## qa-parent overrides

### Enable Static Analysis

By default qa checks do not run, you must enable them on a per-module basis or parent pom.

``` xml
<property>
	<bt.qa.skip>false</bt.qa.skip>
</property>
```

### Checkstyle

Refer to [Checkstyle plugin](https://maven.apache.org/plugins/maven-checkstyle-plugin/checkstyle-mojo.html) for all override details.

#### Skip Checkstyle

``` xml
<property>
	<checkstyle.skip>true</checkstyle.skip>
</property>
```

#### Change or Add Checkstyle Rules

The checkstyle config defaults to `bordertech/bt-checkstyle.xml`. This config is based on `sun-checks.xml` provided by checkstyle.

To add or change a checkstyle rule you are required to create your own [config.xml](http://checkstyle.sourceforge.net/config.html) file and set the `checkstyle.config.location` property.

``` xml
<property>
	<checkstyle.config.location>${basedir}/my-checkstyle-config.xml</checkstyle.config.location>
</property>
```

#### Ignore Checkstyle Rule

Create a [suppression](http://checkstyle.sourceforge.net/config_filters.html) file add set the `checkstyle.suppressions.location` property.

``` xml
<property>
	<checkstyle.suppressions.location>${basedir}/my-suppression.xml</checkstyle.suppressions.location>
</property>
```

Example suppression file:-
``` xml
<?xml version="1.0"?>
<!DOCTYPE suppressions PUBLIC "-//Puppy Crawl//DTD Suppressions 1.0//EN" "http://www.puppycrawl.com/dtds/suppressions_1_0.dtd">
<suppressions>
	<!-- Ignores for all files -->
	<suppress checks="NPathComplexity" files="."/>
</suppressions>
```

### PMD and CPD

Refer to [PMD plugin](https://maven.apache.org/plugins/maven-pmd-plugin/) for all override details.

#### Skip PMD and CPD

``` xml
<property>
	<pmd.skip>true</pmd.skip>
	<cpd.skip>true</cpd.skip>
</property>
```

#### Change PMD Rule Set

The default rule set is `bordertech/bt-pmd-rules.xml`.

You can override the default by creating your own custom [rule set](https://pmd.github.io/latest/pmd_userdocs_making_rulesets.html) and set the `bt.pmd.rules.file` property.

``` xml
<property>
	<bt.pmd.rules.file>${basedir}/my-rules.xml</bt.pmd.rules.file>
</property>
```

#### Add extra PMD Rule set

An extra [rule set](https://pmd.github.io/latest/pmd_userdocs_making_rulesets.html) can be added via the plugin `rulesets` configuration.

``` xml
<plugin>
	...
	<configuration>
		<rulesets>
			<ruleset>${bt.pmd.rules.file}</ruleset>
			<ruleset>${basedir}/my-rules.xml</ruleset>
		</rulesets>
	</configuration>
	...
</plugin>
```

#### Ignore PMD Rule

Create an [excludes](https://pmd.github.io/latest/pmd_userdocs_suppressing_warnings.html#xpath-and-regex-message-suppression) file that lists classes and rules to be excluded from failures and set the `pmd.excludeFromFailureFile` property.

``` xml
<property>
	<pmd.excludeFromFailureFile>${basedir}/my-pmd-excludes.properties</pmd.excludeFromFailureFile>
</property>
```

Example properties file:

``` java
com.my.example.MyClass=LoggerIsNotStaticFinal
```

### Spotbugs

Refer to [spotbugs plugin](https://spotbugs.github.io/spotbugs-maven-plugin/spotbugs-mojo.html) or [doco](https://spotbugs.readthedocs.io/en/latest/index.html) for all override details.

#### Skip spotbugs

``` xml
<property>
	<spotbugs.skip>true</spotbugs.skip>
</property>
```

#### Ignore Spotbugs Rule

Create a [filter](https://spotbugs.readthedocs.io/en/latest/filter.html) file and set `spotbugs.excludeFilterFile` property.

``` xml
<property>
	<!-- List of exclude filter files -->
	<spotbugs.excludeFilterFile>${basedir}/my-spotbugs-exclude-file.xml</spotbugs.excludeFilterFile>
</property>
```

Example filter file:-

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<FindBugsFilter>
	<!-- False Positive on Loading Property Filenames -->
	<Match>
		<Bug pattern="PATH_TRAVERSAL_IN" />
	</Match>
</FindBugsFilter>
```

### OWASP

Refer to [OWASP plugin](https://jeremylong.github.io/DependencyCheck/dependency-check-maven/index.html) for all override details.

#### Skip OWASP

``` xml
<property>
	<dependency-check.skip>true</dependency-check.skip>
</property>
```

#### Ignore OWASP Rule

Create a [suppression](https://jeremylong.github.io/DependencyCheck/general/suppression.html) file add set the `suppression.file` property.

``` xml
<property>
	<suppression.file>${basedir}/my-owasp-suppressions.xml</suppression.file>
</property>
```

#### Using OWASP behind a Proxy

If you are behind a Proxy then the OWASP plugin needs to be told which proxy to use. You can set the `mavenSettingsProxyId` property in your settings.xml to the appropriate PROXY-ID (which is usually defined in the same settings.xml).

``` XML
<properties>
  <mavenSettingsProxyId>MY-PROXY-ID</mavenSettingsProxyId>
</properties>
```

Updating the OWASP vulnerability database can also be blocked by the PROXY blocking HTTP HEAD requests. To work around this you will need a command line option: `-Ddownloader.quick.query.timestamp=false`

### Enforcer Plugin

Refer to [enforcer plugin](https://maven.apache.org/enforcer/maven-enforcer-plugin/enforce-mojo.html) for all override details.

#### Skip enforcer

``` xml
<property>
	<enforcer.skip>true</enforcer.skip>
</property>
```

#### Report issues but dont fail

``` xml
<property>
	<enforcer.fail>false</enforcer.fail>
</property>
```

### Build tools module for override artefacts

If your project has multiple modules and you want to provide the same override/exclude files for all the submodules, then a good solution is to use a `build-tools` module similar to [java-common/build-tools](https://github.com/BorderTech/java-common/tree/master/build-tools). Define a `build-tools` submodule that holds the override/exclude files and then define the plugins you want to override in the project parent pom with the custom build-tools module as a dependency.

``` xml
<!-- Example of adding a custom build-tools module to spotbugs -->
<plugin>
	<groupId>com.github.spotbugs</groupId>
	<artifactId>spotbugs-maven-plugin</artifactId>
	<dependencies>
		<!-- My project build-tools module. -->
		<dependency>
			<groupId>com.my.project</groupId>
			<artifactId>build-tools</artifactId>
			<version>1.0.0-SNAPSHOT</version>
		</dependency>
	</dependencies>
</plugin>
```
