+++
title = "Containers"
+++

> One of the companies we collaborated with for this project where looking into
> using containers to provide a way to host backend for their devices on premise.

# Technology

<!-- TODO is this too opinionated? -->

To understand how to secure something you need to understand a bit about the
technology.

Containers are often explained as a form of light-weight virtual machine (VM).
While this explanation is good enough for an initial understanding (assuming
you learned about virtualization first), it is not really accurate.

> We are going to ignore Windows containers here.
> I know it is a thing, but have yet encounter it in the wild.
> Regardless, Windows containers are fundamentally different on a
> technological level.

## Cgroups & namespaces

What containers really are, is a combination of two features of the Linux
kernel.
Those are [cgroups](https://en.wikipedia.org/wiki/Cgroups) and
[namespaces](https://en.wikipedia.org/wiki/Linux_namespaces).
The <abbr title="Too long; didn't read">TL;DR</abbr> is that namespaces provide
isolation for processes and cgroups allows limiting of resources.
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
