# Jkwee AspectJ Maven Plugin

Forked from [mojohaus's aspectj-maven-plugin](https://github.com/mojohaus/aspectj-maven-plugin). Removed CreateSite and CreateReport functionality due to vulnerabilities in [doxia-site-renderer v1.10](https://github.com/apache/maven-doxia-sitetools/tree/master/doxia-site-renderer). Through OWASP's SCA tool (which is included in this project's pom), the vulnerabilities that were 'solved' are:

* maven-core-3.3.9 [CRITICAL IN SEVERITY]: Apache Maven will follow repositories that are defined in a dependency’s Project Object Model (pom) which may be surprising to some users, resulting in potential risk if a malicious actor takes over that repository or is able to insert themselves into a position to pretend to be that repository. Maven is changing the default behavior in 3.8.1+ to no longer follow http (non-SSL) repository references by default. More details available in the referenced urls. If you are currently using a repository manager to govern the repositories used by your builds, you are unaffected by the risks present in the legacy behavior, and are unaffected by this vulnerability and change to default behavior. See this link for more information about repository management: https://maven.apache.org/repository-management.html

* commons-beanutils-1.7.0 [HIGH IN SEVERITY]: Apache Commons BeanUtils, as distributed in lib/commons-beanutils-1.8.0.jar in Apache Struts 1.x through 1.3.10 and in other products requiring commons-beanutils through 1.9.2, does not suppress the class property, which allows remote attackers to "manipulate" the ClassLoader and execute arbitrary code via the class parameter, as demonstrated by the passing of this parameter to the getClass method of the ActionForm object in Struts 1.

* plexus-i18n-1.0-beta-10 [HIGH IN SEVERITY]: This affects the package i18n before 2.1.15. Vulnerability arises out of insufficient handling of erroneous language tags in src/i18n/Concrete/TextLocalizer.cs and src/i18n/LocalizedApplication.cs.

* velocity-1.7 [HIGH IN SEVERITY]: An attacker that is able to modify Velocity templates may execute arbitrary Java code or run arbitrary system commands with the same privileges as the account running the Servlet container. This applies to applications that allow untrusted users to upload/modify velocity templates running Apache Velocity Engine versions up to 2.2.

* velocity-tools-2.0 [MEDIUM IN SEVERITY]: The default error page for VelocityView in Apache Velocity Tools prior to 3.1 reflects back the vm file that was entered as part of the URL. An attacker can set an XSS payload file as this vm file in the URL which results in this payload being executed. XSS vulnerabilities allow attackers to execute arbitrary JavaScript in the context of the attacked website and the attacked user. This can be abused to steal session cookies, perform requests in the name of the victim or for phishing attacks.

## Noteworthy Changes
* Unable to generate site and reports as those functionalities are directly tied to the doxia-site-renderer dependency.
* Upgraded maven version to support 3.8.1.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Maven Central](https://img.shields.io/maven-central/v/org.codehaus.mojo/aspectj-maven-plugin.svg?label=Maven%20Central)](https://search.maven.org/artifact/org.codehaus.mojo/aspectj-maven-plugin)
[![GitHub CI](https://github.com/mojohaus/aspectj-maven-plugin/actions/workflows/maven.yml/badge.svg)](https://github.com/mojohaus/aspectj-maven-plugin/actions/workflows/maven.yml)

## Overview

This plugin weaves AspectJ aspects into your classes using the AspectJ compiler `ajc`.
Typically, aspects are used in one of two ways within your Maven reactors:

  * As part of a Single Project, implying aspects and code are defined within the same Maven project.
    This is the simplest approach to start out with; feel free to examine the
    "Examples: Single-project AspectJ use" to better understand single-project use.

  * As part of a Multi-module Maven Reactor where one/some project(s) contains aspects and other
    projects within the Maven reactor contain code using the aspects ("woven by the aspects").
    This is a more complex and powerful approach, best suited when several Maven projects should be woven
    by a common set of aspects. The "Examples: Multi-module AspectJ use" contains a basic walk-through
    of this approach.

## Contributing

The first step is to create [an appropriate issue](https://github.com/mojohaus/aspectj-maven-plugin/issues). Describe the problem/idea you have and create an appropriate pull request.

Test you changes locally using 

```shell
mvn clean verify -Pdocs,run-its
```

If you need to contact a committer, please consider getting active [on the mailing lists](https://groups.google.com/forum/#!forum/mojohaus-dev).


## Releasing

* Make sure `gpg-agent` is running.
* Make sure all tests pass `mvn clean verify -Prun-its`
* Execute `mvn -B release:prepare release:perform`

For publishing the site do the following:

```
cd target/checkout
mvn verify site site:stage scm-publish:publish-scm
```
