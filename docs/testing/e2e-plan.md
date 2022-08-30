# Curated Packages End-To-End Testing Plan

This document summarizes the current state and future plans for end-to-end (E2E) testing in the EKS-A Curated Packages (CP) project.

## The Present

End-to-end tests can be written to exercise CP workflows. Several parameters can be selected to control the environment (cluster) in which a test runs. These include: EKS-A provider, node OS, and Kubernetes (K8s) version. For examples, see [TestCPackagesHarborInstallSimpleFlow](https://github.com/aws/eks-anywhere/blob/de84797d702028e15deb3363bbe222489f00229d/test/e2e/harbor_test.go) and [TestCPackagesVSphereKubernetes121SimpleFlow](https://github.com/aws/eks-anywhere/blob/de84797d702028e15deb3363bbe222489f00229d/test/e2e/curated_packages_test.go#L102).

Tests have been written that cover the installation and running of the CP packages controller and the [hello-eks-anywhere test application](https://github.com/aws/eks-anywhere-build-tooling/blob/61628f1669044a39ff561385c7032e3b6c7d12ed/projects/aws-containers/hello-eks-anywhere/conf/hello.sh) across the three dimensions mentioned above (EKS-A provider, node OS, and K8s version). Additional tests have been written to exercise Harbor and MetalLB package installations. The results of these tests are reported in the appropriate EKS-A communications channels.

*But the existing tests are not nearly enough to provide confidence that our packages are bug-free.* The current CP E2E framework does not (without significant effort by the test writer) run the existing tests against the versions of their artifacts that matter most. Nor do they run them for each submitted pull request (PR). These things are not the fault of the tests themselves, nor the tests' writers, but rather they've exposed the limitations of the current CP E2E framework.

## The End-Goal

To be able to provide confidence that the CP offerings are working across our supported providers, OSes, and K8s versions, the current framework must be improved in the following ways:

+ **Artifact version control:**
  Tests should be run against the latest versions of artifacts. [??Bonus points for letting test writers specify specific versions.??]

+ **Timing:**
  E2E tests should be run on every revision of an "ok to test" PR.

+ **Platform configuration:**
  A test should be able to exclude specific providers, OSes, or K8s versions with which they are incompatible or unsupported.
  
+ **Configuration review & control:**
  The test dimensions should be easily viewed, verified, and edited by humans (or reasonable facsimiles thereof). This provides a means of quickly excluding tests categorically for reasons such as hardware outage.

Additionally, it is foreseen that the number of E2E tests that need to be run for each PR will quickly accumulate, and so it is recommended that these performance goals also be considered:

+ **Reusable clusters:** The amount of time required to create and destroy clusters before and after each test will quickly become overwhelming with reasonable amounts of hardware.

## The Path Forward

### Short Term

#### Test Against Current Branch

The current tests can be modified to run against the latest versions of artifacts, and the build-tooling can be configured to access those versions.

#### Test On Each Pull Request

Can we enable these tests to run on each PR with the current system?

### Long Term

Separate our build tooling?

