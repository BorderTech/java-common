# Change log

## Release in-progress
* Update to latest plugin dependencies
* Latest plugin dependencies now have a minimum maven version of 3.6.3.
* Removed ReturnEmptyArrayRatherThanNull pmd rule that is no longer supported.

## 1.0.20
* Update plugin dependencies in bordertech_parent
* Update plugin dependencies in qa_parent
* Made the following modifications to CheckStyle config file bt-checkstyle.xml:
  * Moved tabWidth override to the top level checker element
  * Changed tab indention check to no longer allow any violations by removing the maximum property which was set to 10
  * Changed JavadocPackage check to warning
  * Changed EmptyForIteratorPad check to warning as IntelliJ code formatting does not currently allow for this
* Added a new checkstyle config file bt-checkstyle-format-only.xml for projects that only want to check basic code formatting.
* Modified the jacoco report goal to the test phase and to make sure it runs after surefire, moved the jacoco plugin after the surefire plugin in the pom file which is how maven determines the run order of plugins defined on the same phase #88

## 1.0.19
* Added bt.qa.fail property as a convenience property to allow projects to fail builds or only report QA violations. #84
* Added bt.convergence.check.fail property to allow the convergence check to be set to fail without overriding the enforcer plugin in bordertech_parent. #84
* Update PMD and Checkstyle to latest versions

## 1.0.18
* Default QA plugins to ignore generated code #61
* Update OWASP plugin skip property default to use bt.qa.skip #79
* Move enforcer convergence check into verify phase. Can be skipped using bt.convergence.check.skip=true property. #78
* Move versions-maven-plugin into a profile display-versions to allow projects to opt in or out #74
* Switch from travis-ci to GitHub Actions #75

## 1.0.17
* Update plugins to latest version

## 1.0.16
* Add quick-build profile that skips tests and QA checks #63
* Change OWASP checker to fail on Critical (Level 9-10) issues and check for updates every 12 hours #60
* Update dependencies and plugin versions.
* Update bt-checkstyle.xml to include latest checks from checkstyle's sun_check.xml.
* Plugin configuration is only done via user properties which is easier for projects to override #59

## 1.0.15
* Refactor README #52
* Update plugin versions #54

## 1.0.14
* Update README.
* Latest checkstyle and pmd.

## 1.0.13
* Remove redundant ${bt.plugin.xxxx} properties. Use plugin user properties instead.
* Update QA override details in README.
* Update bt-checkstyle.xml to include the latest checkstyle sun-check.xml changes.
* Delete bt-spotbugs-exclude-filter.xml as projects should handle their own excludes.
* Introduce ${bt.version} properties for dependency versions.
* Introduce maven version checker to display updates for project dependencies.
* Move enforce dependency convergence to qa-parent

## 1.0.12
* Updated plugin versions.
* Enhance vulnerability checking.
* Minor fixes to README.

## 1.0.11
* Update OWASP properties.

## 1.0.10
* Include dependency convergence check in the maven enforcer plugin.

## 1.0.9
* Fix Jacoco Coverage Report.

## 1.0.8
* Latest rules and versions of checkstyle, pmd and spotbugs (formerly findbugs).
* Removed wc.qa.skip property to only use bt.qa.skip.
* Removed surefire.version property as use plugin inheritance for version.
* Upgrade to junit 5.

## 1.0.7
* Update version of dependency-check-maven and change default config.
* Remove site generation.

## 1.0.6
* Added properties to manage non-java analysers in the dependency check plugin (see wiki).
* Turned off the following analysers (in default configuration):
  * nsp analyzer;
  * nuspec analyzer;
  * swift package manager analyzer; and
  * assembly analyzer.
* Incremented version of dependency-check-maven to 3.3.2.

## 1.0.5
* Added support for OWASP dependency checker using dependency-check-maven.

## 1.0.4
* Update README.
* Added bt.qa.skip property.
* Fix badger version.

## 1.0.3
* Added qa-parent.

## 1.0.2
* Generate javadoc and sources in release.

## 1.0.1
* Surefire version property.

## 1.0.0
* Initial version
