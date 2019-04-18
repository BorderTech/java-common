# java-common

Reusable build configuration for for BorderTech open source projects.

## Status

[![Build Status](https://travis-ci.com/BorderTech/java-common.svg?branch=master)](https://travis-ci.com/BorderTech/java-common)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/c7a2226acd574943af9ae966c54b05e6)](https://app.codacy.com/app/BorderTech/java-common?utm_source=github.com&utm_medium=referral&utm_content=BorderTech/java-common&utm_campaign=Badge_Grade_Dashboard)
[![Maven Central](https://img.shields.io/maven-central/v/com.github.bordertech.common/bordertech-parent.svg?label=Maven%20Central)](https://search.maven.org/search?q=g:%22com.github.bordertech.common%22%20AND%20a:%22bordertech-parent%22)

## qa-parent

BorderTech java projects should generally use this as their parent POM.

It runs quality assurance checks on your java code using tools such as checkstyle, pmd and findbugs.

By default qa checks do not run, you must enable them on a per-module basis in the pom.xml like so:

``` xml
<properties>
  <!-- Set bt.qa.skip to false to run QA checks. -->
  <bt.qa.skip>false</bt.qa.skip>
</properties>
```

Refer to qa-parent's [pom.xml](https://github.com/BorderTech/java-common/blob/master/qa-parent/pom.xml) for other project properties.

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

## QA Overrides

### Enforcer Plugin - [info](https://maven.apache.org/enforcer/maven-enforcer-plugin/enforce-mojo.html)

How to skip enforcer:-
```
<property>
	<!-- Report issues but dont fail -->
	<enforcer.fail>false</enforcer.fail>
	<!-- Skip -->
	<enforcer.skip>true</enforcer.skip>
</property>
```

### Skip ALL Static Analysis

How to skip ALL analysis tools:-
```
<property>
	<bt.qa.skip>false</bt.qa.skip>
</property>
```

`bt.qa.skip` property sets the individual skip properties for all analysis tools.

### Checkstyle - [info](https://maven.apache.org/plugins/maven-checkstyle-plugin/checkstyle-mojo.html)

How to skip Checkstyle:-
```
<property>
	<checkstyle.skip>true</checkstyle.skip>
</property>
```

How to override QA Checkstyle defaults:-

```
<property>
	<checkstyle.consoleOutput>true</checkstyle.consoleOutput>
	<checkstyle.failOnViolation>true</checkstyle.failOnViolation>
	<checkstyle.linkXRef>false</checkstyle.linkXRef>
	<checkstyle.includeTestSourceDirectory>false</checkstyle.includeTestSourceDirectory>
</property>
```

#### Add Checkstyle Rule
```
<property>
	<!-- Default config location -->
	<bt.checkstyle.config.file>bordertech/bt-checkstyle.xml</bt.checkstyle.config.file>
</property>
```

#### Remove Checkstyle Rule

### PMD and CPD - [info](https://maven.apache.org/plugins/maven-pmd-plugin/)

How to skip PMD and CPD:-
```
<property>
	<pmd.skip>true</pmd.skip>
	<cpd.skip>true</cpd.skip>
</property>
```

How to override QA PMD and CPD defaults:-

```
<property>
	<!-- Priority: 1 (High) - 5 (Low) -->
	<bt.pmd.failure.priority>2</bt.pmd.failure.priority>
	<bt.pmd.min.priority>3</bt.pmd.min.priority>
</property>
```

NOTE - The standard properties of the plugin can be set instead of the bordertech defaults.

#### Add PMD Rule
```
<property>
	<bt.pmd.rules.file>bordertech/bt-pmd-rules.xml</bt.pmd.rules.file>
</property>
```

#### Remove PMD Rule


### Spotbugs - [info]()

How to skip:-
```
<property>
	<spotbugs.skip>true</spotbugs.skip>
</property>
```

How to override QA Spotbug defaults:-

```
<property>
	<!-- Rank: Scariest (1-4), Scary (5-9), Troubling (10-14), Of concern (15-20) -->
	<bt.spotbugs.rank>14</bt.spotbugs.rank>
	<!-- Threshold Confidence: High, Medium, Low -->
	<bt.spotbugs.threshold>Medium</bt.spotbugs.threshold>
</property>
```

NOTE - The standard properties of the plugin can be set instead of the bordertech defaults.

#### Add Spotbugs Rule

```
<property>
	<bt.spotbugs.exclude.file>bordertech/bt-spotbugs-exclude-filter.xml</bt.spotbugs.exclude.file>
	<bt.spotbugs.exclude.files>${bt.spotbugs.exclude.file}</bt.spotbugs.exclude.files>
</property>
```


#### Remove Spotbugs Rule


### OWASP - [info](https://jeremylong.github.io/DependencyCheck/dependency-check-maven/configuration.html)

How to skip OWASP:-
```
<property>
	<bt.owasp.skip>false</bt.owasp.skip>
</property>
```

How to override OWASP defaults:-

```
<property>
	<!-- Min cvss score to fail on. Range 0-10 : LOW: 0-3.9, MEDIUM: 4-6.9, HIGH: 7.0-8.9, Critical: 9.0-10.0 -->
	<bt.owasp.fail.cvss.min>0</bt.owasp.fail.cvss.min>
	<!-- If true, override min cvss and fail on any vulnerability. -->
	<bt.owasp.fail.any>false</bt.owasp.fail.any>
	<!-- If set, owasp uses the proxy id in maven settings to download its db. -->
	<bt.owasp.proxy.id />
</property>
```

NOTE - The standard properties of the plugin can be set instead of the bordertech defaults.

#### Add OWASP Rule

#### Remove OWASP Rule
