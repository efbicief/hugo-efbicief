---
title: "Release management"
date: 2022-05-20T10:42:34+01:00
draft: false
tags: [ "CS261", "Software Engineering", "Notes" ]
---
# Version control
- Protects code as delivery nears
- Mitigates human and technical errors
- Generally maintain 3 releasable environments: dev, feature and master

# System building
The codebase is only properly linked together towards the end of development. We use various tools to help us.
- Build script generation
- Version control system integration, to get correct component versions
- Minimal recompilation: Only recompile changed modules
- Executable system creation: Easy setup/installer
- Test automation at build time
- Reporting (on build progress/success/failure)
- Documentation generation

# Integrating environments
- Merging dev systems, since each developer will have a slightly different version
- Build server should maintain the definitive development build
- Building for target environment, including databases and apps not available in the development target

Larger projects take longer to build, so may need less frequent builds with more changes each build.

# Dummy and production data
When building for production, you need to switch from dummy data sources to real (production) data. To do so, you should check:
- Availability of data sources
- Overhead of data processing
- Long term storage costs
- Data sharing and access constraints

# Releasing
- Releasing is expensive - consider configuration, installation, documentation, marketing, etc.
- May need installation support if targeted for a variety of devices
- Updates should consider updating with missed updates, and package possible extra changes (eg. going from v1.0 -> v3.0 should also package v2.0 changes)
- Release checklist:
  - Explicit dependencies
  - Distributed release, via many channels
  - Minimal effort for customers to install
  - Keep control of release scope (who can install)
  - Keep a release log and descriptive update information
  - Prevent unnessecary retrievals for updates

# Post-release updates
Consider:
- Cost of making a change
- benefits of making a change
- Drawbacks of *not* making a change
- Affected users
- Product release cycle- agile methodologies nessecitate updates as part of the release cycle
