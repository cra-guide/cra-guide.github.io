+++
title = "Software Bill of Materials"
+++

A Software Bill of Materials (SBOM) goes into producing a piece of software.
For a .NET project this will be the framework version and exact versions of all
NuGet packages used to produce the build.
For a firmware image of an embedded system, an SBOM consist of bootloader,
libraries, executables and file that make up the image.

SBOMs can have various uses.

- Identify vulnerabilities in components
- Track compromised dependencies
- Ensure license compliance (MIT, GPL, Apache etc.)
- Component transparency

> identify and document vulnerabilities and components contained in products
> with digital elements, including by drawing up a software bill of materials
> in a commonly used and machine-readable format covering at the very least the
> top-level dependencies of the products;

Source [ANNEX I](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847#anx_I)

> If the manufacturer decides to make available the software bill of materials
> to the user, information on where the
software bill of materials can be accessed.

Source [ANNEX II](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847#anx_II).

The manufacturer is not required to make the SBOM public.
But they might need to make it available to authorities upon request.

## CycloneDX vs SPDX

The two commonly used formats for machine-readable SBOM are CycloneDX
(ECMA-424) and SPDX (ISO/IEC 5962:2021).

## Generating SBOMs

Most tools for generating SBOM will include transient dependencies in addition
to top-level dependencies.
Though including transient dependencies is not required, the inclusion
increases the usefulness of the SBOM, as it allows scanning all
dependencies against databases of known vulnerabilities.

SBOM shall be generated for all releases.
It is common to set up SBOM generation as part of CI/CD pipeline.
Here is a list of SBOM generation tools for some popular technologies.

### NPM

- [CycloneDX SBOM for npm](https://github.com/CycloneDX/cyclonedx-node-npm)

### Python

- [CycloneDX Python SBOM Generation Tool](https://github.com/CycloneDX/cyclonedx-python)
- [pip-audit (CycloneDX)](https://github.com/pypa/pip-audit)

### .NET

- [SBOM tool (SPDX)](https://github.com/microsoft/sbom-tool)
- [CycloneDX module for .NET](https://github.com/CycloneDX/cyclonedx-dotnet)

### Java

- [CycloneDX Maven Plugin](https://github.com/CycloneDX/cyclonedx-maven-plugin)
- [CycloneDX Gradle Plugin](https://github.com/CycloneDX/cyclonedx-gradle-plugin)

### Rust

- [CycloneDX Rust (Cargo) Plugin](https://github.com/CycloneDX/cyclonedx-rust-cargo)

### Containers

- [Syft (CycloneDX+SPDX)](https://github.com/anchore/syft)
  - Often used in combination with [Grype](https://github.com/anchore/syft)
    for vulnerability scanning.
- [Trivy](https://trivy.dev/docs/latest/guide/supply-chain/sbom/)
- [Docker Scout](https://docs.docker.com/dhi/core-concepts/sbom/)
- [Docker Buildkit](https://docs.docker.com/build/metadata/attestations/sbom/)

### Various

- [CycloneDX Generator (cdxgen)](https://github.com/cdxgen/cdxgen)

For more SBOM related tools, see [CycloneDX Tool
Center](https://cyclonedx.org/tool-center/).

### Embedded Linux

- [CycloneDX Buildroot](https://github.com/CycloneDX/cyclonedx-buildroot)
- [Yocto - Creating a Software Bill of Materials (SPDX)](https://docs.yoctoproject.org/next/dev-manual/sbom.html)

## Analyzing SBOM

- [OWASP Dependency-track](https://dependency-check.github.io/DependencyCheck/)
