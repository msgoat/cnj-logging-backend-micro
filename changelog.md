# Changelog
All notable changes to `cnj-logging` will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [5.1.0] - 2023-11-14
### Added
- added missing dependency on assertj for testing
- Tagging of git branch
### Changed
- Upgraded to helm-maven-plugin version 5.0.0
- Now a helm chart is packaged and pushed as an artifact during the commit-stage build
- Now the helm chart is pulled before deploying during the integration-test-stage build
- removed dependency on cnj-common-test-jakarta by switching to model based system tests
- upgraded Payara version to 6.2023.10
- consolidated dependencies

## [5.0.0] - 2023-06-09
### Changed
- moved to new AWS CodeBuild build pipeline
- moved to new CloudTrain EKS cluster
- upgraded everything
- added docker-compose.yml to run the showcase locally

## [4.3.0] - 2023-02-24
### Changed
- minor changes to make build on AWS CodeBuild work

## [4.1.0] - 2021-04-22
### Added
### Changed
- performed lift&shift to new AWS CloudTrain environment
- upgrade to latest version of runtime environments and servers pending
