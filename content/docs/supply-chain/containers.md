+++
title = "Containers"
+++

> This came about because one of the companies we collaborated with for this
> project, were looking into using containers to provide a way to host backend
> for their devices on premise.

# Technology

> We are only going to consider Linux containers on this page, primarily
> focusing on Docker, since it is the industry standard these days.

To understand how to secure something you need to understand a bit about the
technology.

Containers are often explained as a form of light-weight virtual machine (VM).
While this explanation is good enough for an initial understanding (assuming
prior knowledge of virtualization), it is not the full story.

## Cgroups & namespaces

What containers really are, is a combination of two features of the Linux
kernel.
Those are [cgroups](https://en.wikipedia.org/wiki/Cgroups) and
[namespaces](https://en.wikipedia.org/wiki/Linux_namespaces).
The short version is that namespaces provide isolation for processes and
cgroups allows limiting of resources.
So all containers are, is just regular processes (or process trees) with
isolation and resource control.
Therefore, instead of thinking about containers as "light-weight VMs", it is
more useful in a security context to think of them as sandboxes.

Great care should be taking when deploying containers with something like
Docker Engine, as it is very easy to misconfigure.
Docker is not really secure by default.
Even with all the appropriate security measures, isolation is not as strong as
VMs.
In fact some cloud providers (AWS & Fly) "secretly" deploy containers to micro
VMs using [Firecracker](https://github.com/firecracker-microvm/firecracker).

## Images

Containers are commonly distributed as [OCI](https://opencontainers.org/)
compliant containers images.
They consist of some metadata known as [Image
Manifest](https://github.com/opencontainers/image-spec/blob/main/manifest.md)
and a [filesystem
changeset](https://github.com/opencontainers/image-spec/blob/main/layer.md).
You have probably heard about layers in docker.
When writing a Dockerfile, each instruction becomes a new layer when building.
A layer is really a [tar
archive](<https://en.wikipedia.org/wiki/Tar_(computing)>), optionally compressed
with either [zstd](https://en.wikipedia.org/wiki/Zstd) or
[gzip](https://en.wikipedia.org/wiki/Gzip).
The files in a layer archive represents changes to a filesystem (add, update &
remove/whiteout).
All the layers for an image stacked on top of each other provides the initial
root filesystem for a container.
A tool like [dive](https://github.com/wagoodman/dive) can be used to inspect
the content of each layer.

This is important because anything added in a layer can always be extracted
from an image, even if the file was removed in another layer.
It might be tempting to add secrets (encryption keys etc) in a layer, but you
should know that they will then be distributed as part of the images.
It is therefore strongly discouraged to reference secrets in Dockerfiles.

Good security requires defense in depth, that is multiple layers of defenses
working in depth.
When deploying with containers it is of cause important to secure the application.
The <abbr title="potential scope of damage">blast radius</abbr> should be kept
as small as possible in case a container gets compromised.
Care should be taken not to leak secrets by accidentally baking them into
container images.

# Harden containers

## Base image

Using something familiar (Ubuntu, Debian etc) as base image for containers is
tempting.
It has been the recommendation for a while now, to use smaller such as Alpine
for base image.
Even though the Alpine image is small, it still contains plenty of tools that
attackers to do damage.
There is a [busybox](https://en.wikipedia.org/wiki/BusyBox) including `nc` (aka
[netcat](https://en.wikipedia.org/wiki/Netcat)) commonly used by attackers to
create a backdoor.
They can also just install whatever tool they want with `apk`.

Using a base image with lots of tools that makes it convenient for debugging,
also makes it convenient for attackers.

Compilers and SDKs are needed for building the application, but aren't needed
to run it.
It is therefore recommended to use [multi-stage
build](https://docs.docker.com/build/building/multi-stage/).

A [distroless](https://docs.docker.com/dhi/core-concepts/distroless/) image
should be used for the final build.
Distroless images don't have debugging tools, package managers, shells and
other common tools.
Using a distroless base image for the final container image, severely limits an
attackers' ability to do [lateral
movement](https://www.cloudflare.com/learning/security/glossary/what-is-lateral-movement/).

## Attestation

TODO verify signature of image. Cosign + docker scout

## Scanning

There are a number of tools that can do vulnerability scanning for containers.
The official solution for Docker is [Docker
Scout](https://docs.docker.com/scout/) (requires subscription).
Two other options we've come a cross a lot are:
[Trivy](https://github.com/aquasecurity/trivy),
[Grype](https://github.com/anchore/grype) and
[Snyk](https://docs.snyk.io/developer-tools/snyk-ci-cd-integrations/github-actions-for-snyk-setup-and-checking-for-vulnerabilities/snyk-docker-action).

There are many other tools/vendors that act in this area. They vary in the
capabilities.
So it is advisable to do some research.
Some of the features container scanning tools might provide is:

- Scan image for known vulnerability
- Detect some misconfiguration
- Find secrets baked into images
- Software license compliance

Trivy is an open source container image scanner with all the aforementioned
capabilities.
It might be a good place to start to get familiar with these kinds of tools.
Though it is worth noting that [Trivy has been involved in a couple of security
incidents](https://thehackernews.com/2026/03/trivy-security-scanner-github-actions.html).

See also [Security Testing](security-testing.md).

# Runtime hardening

## Non-root user

By default, Docker run commands inside containers as root.
It is therefore advisable to specify another user when running containers.
There are a couple of different ways this can be done.

1. When running a container, you can specify a UID with `-u` options.
   Example: `docker run -u 4000 alpine`.
2. When writing a Dockerfile, a non-root should be specified with the [USER
   instruction](https://docs.docker.com/reference/dockerfile/#user).
3. By enabling [User namespace
   remapping](https://docs.docker.com/engine/security/userns-remap/#enable-userns-remap-on-the-daemon)
   on Docker daemon.
4. By enabling [Enhanced Container
   Isolation](https://docs.docker.com/enterprise/security/hardened-desktop/enhanced-container-isolation/)
   (requires subscription).

## Availability

Availability is an important security goal.
In addition to preventing access to unauthorized users, it is just as important
that authorized users can access the service.

For any long-lived process, there is a chance that at some point, it will
become unresponsive.
The first step for solving this issue for containers, is to be able to detect
that a container has become "unhealthy".
Unhealthy - meaning that the container has stopped responding to requests.
This is done with healthcheck/liveness probes.
The common strategy for dealing with these issues, is simply to restart the
container when it stops responding.

How health check/liveness probes are declared, depends a bit on how you run the
container.

For **Docker**, it is called healthcheck.
And there are two ways to declare such a check.
Either with the [HEALTHCHECK
instruction](https://docs.docker.com/reference/dockerfile/#healthcheck) in
Dockerfile.
Or with the [healthcheck
attribute](https://docs.docker.com/reference/compose-file/services/#healthcheck)
in Compose file.

It is important that you carefully consider how to implement the health check,
as you don't want it to become an attack vector. Healthchecks in Docker runs a
command (often `curl`) inside the container. Including `curl`, `nc` etc. in
your service container image, also provides a tool that can be abused by
attackers.

For **Kubernetes**, use the [liveness
command](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/).

## Monitoring

There are a plethora of monitoring services such as:
[Middleware](https://middleware.io/product/container-monitoring/),
[sematext](https://sematext.com/), [datadog](https://www.datadoghq.com/),
[Splunk](https://www.splunk.com/) etc.
However, these can be pricey.

For a self-hosted monitoring solution, you can use
[Prometheus](https://prometheus.io/) plus either
[Grafana](https://grafana.com/) or [Perses](https://perses.dev/).

Support for observability beyond simple healthcheck probes can be added by
implementing [OpenTelemetry standard](https://opentelemetry.io/).
It is important though, that you very carefully read the [documentation on
security](https://opentelemetry.io/docs/security/), as it can otherwise leave
new attack vectors open and break regulatory compliance such as GDPR.

## Secrets

It is important to be aware that any secrets set with `ENV` or `ARG` in a
Dockerfile will persist in the final image.
So setting secrets that way is strongly discouraged.

Using secrets with Docker is done via mounts.
See [Build secrets](https://docs.docker.com/build/building/secrets/) and
[Secrets in Compose](https://docs.docker.com/compose/how-tos/use-secrets/).

Use a scanning tool to check for secrets in your images.
See [Scanning](#scanning).

Cloud providers generally provide their own way of managing secrets for
containers.
See the documentation for your provider for details.

## Secure Computing (seccomp)

Docker integrates with seccomp, which is a feature of the Linux kernel,
restricting which system calls can be made by a process/container.
See [Seccomp security profiles for
Docker](https://docs.docker.com/engine/security/seccomp/).

## Further reading

Be sure carefully read the official
[dockerdocs](https://docs.docker.com/manuals/) as security advice is scattered
throughout.
Pay special attention to the [Docker Engine
Security](https://docs.docker.com/engine/security/) section.

We also recommend reading [OWASP Docker Security Cheat
Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html).
