+++
title = "Containers"
+++

> This came about because one of the companies we collaborated with for this
> project, were looking into using containers to provide a way to host backend
> for their devices on premise.

# Technology

<!-- TODO is this too opinionated? -->

To understand how to secure something you need to understand a bit about the
technology.

Containers are often explained as a form of light-weight virtual machine (VM).
While this explanation is good enough for an initial understanding (assuming
prior knowledge of virtualization), it is not the full story.

> We are going to ignore Windows containers here.
> I know it is a thing, but have yet encounter it in the wild.
> Regardless, Windows containers are fundamentally different on a
> technological level.

## Cgroups & namespaces

What containers really are, is a combination of two features of the Linux
kernel.
Those are [cgroups](https://en.wikipedia.org/wiki/Cgroups) and
[namespaces](https://en.wikipedia.org/wiki/Linux_namespaces).
The short version is that namespaces provide isolation for processes and
cgroups allows limiting of resources.
So all containers are, is just regular processes (or process trees) with
isolation and resource control.
Therefore, instead of thinking about containers as "light-weight VMs" it is
probably more useful (in a security context) to think of them as sandboxes.

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
