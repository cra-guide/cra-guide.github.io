+++
title = "Development Environment"
+++

## Introduction

The main advice regarding security of supply chain have long been to keep
dependencies up to date.
Either by running regular audits with designated package manager.
Or using something like
[Dependabot](https://docs.github.com/en/code-security/tutorials/secure-your-dependencies/dependabot-quickstart-guide).

Given the [growing malware
trend](https://www.endorlabs.com/learn/npm-account-takeovers-are-a-growing-malware-trend)
packages on npm and other registries, it is time we start treating them as
dangerous.
Developers are the target for much of this.

A recent version of pnpm have implemented some [supply-chain
protection](https://pnpm.io/blog/releases/11.0#highlights) to lower the risk of
npm based malware infection.

Another precaution to limit the impact of such attacks are to use sandboxed
development environments.
One solution is to use cloud development environments (CDE), such as: [GitHub
Codespaces](https://github.com/features/codespaces), [Google Cloud
Workstations](https://cloud.google.com/workstations) and [AWS
Cloud9](https://aws.amazon.com/cloud9/).
Another is to run your own virtual machine for development.

Using isolated development environments are also an advised safety precaution
for agentic coding.

## Custom virtual machine

Using custom virtual machines for development environments is an option for
organizations that already have the necessary infrastructure, and want tight
control of costs.

Our recommendation is that the organization create a VM template per
tech-stack.

## Access tokens

You need to be able to push commits from your sandboxed development environment.
We recommend setting up branch protection to force review of all code changes
before merging into the main branch.

On GitHub, we recommend using [fine-grained personal access tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#fine-grained-personal-access-tokens).
The [principle of least
privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege) should
be followed when creating tokens.
They should be scoped to only the necessary repository.
In addition, tokens can be created with a short expiration, though that can
create some inconvenience.

## CI/CD

Developers should not be allowed to publish artifacts themselves.
This should only be allowed from a protected CI/CD environment.

Some of the aforementioned supply-chain malware spread by exploiting the CI/CD
pipeline.
If you or your organization is maintainer of open source packages, then it
might be a good idea to have a security conscious person review CI/CD pipeline.
Look for and avoid ways where untrusted code can run as part of the pipeline,
because that could cause unauthorized exposure of access tokens.
