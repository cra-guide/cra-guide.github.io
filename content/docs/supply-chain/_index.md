+++
title = "Securing the supply chain"
weight = 20
+++

<!-- TODO check that this is actually in line with CRA -->

> We have deliberately limited this section to only be concerned with software
> supply chain security.
> Information on physical components and materials should be sought after
> elsewhere.

## Introduction

Manufactures of products with digital elements (products with software) needs
to follow due diligence when it comes to components in its supply chain.

It is a concrete requirement that manufactures maintain a [SBOM (Software Bill
of Material)](sbom).

Products must not be put on the marked if they include known exploitable
vulnerabilities.
This requirement extends to the components used on the software.
In this context components refer modules, libraries and frameworks etc. that
are included as part of the software.
It includes everything you get from package repositories such as
[NPM](https://www.npmjs.com/), [PyPI](https://pypi.org/),
[NuGet](https://www.nuget.org/) etc.
This is important because most (if not all) software these days are build on
the shoulder of giants.
Meaning we don't re-invent the wheel for every single thing.
We use frameworks, plugins and libraries which inherently become part of our
product.
We therefore need due diligence when it comes to selecting and maintaining
those components.

Manufactures must provide security updates free of charge for the lifetime of
the product.
Which is at least 5 years, or however long a product of a given category can
reasonably be expected to last.
These updates should (where applicable) be installed automatically.
Security updates should be delivered separate from feature updates/changes.
And there should be a clear opt-out mechanism for automatic updates.

## Supply threats

<abbr title="European Union Agency for Cybersecurity">ENISA</abbr> ranks
"Supply Chain Compromise of Software Dependencies" as the nr 1 threat in their
[Foresight Cybersecurity Threats For
2030](https://www.enisa.europa.eu/publications/foresight-cybersecurity-threats-for-2030-update-2024-extended-report)
report.

In 2025 and early 2026 we've seen some prominent examples of worms spreading
through software dependencies, such as [Shai
Hulud](https://www.bleepingcomputer.com/news/security/shai-hulud-malware-infects-500-npm-packages-leaks-secrets-on-github/),
[Shai Hulud
2.0](https://www.bleepingcomputer.com/news/security/shai-hulud-20-npm-malware-attack-exposed-up-to-400-000-dev-secrets/)
and
[Glassworm](https://www.bleepingcomputer.com/news/security/glassworm-malware-hits-400-plus-code-repos-on-github-npm-vscode-openvsx/).
Besides these worms, we've seen a number of prominent supply chain attacks in recent years.
These are just some of the stories.

- [SolarWinds Compromise (SUNBURST) - 2020](https://www.breachsense.com/blog/solarwinds-data-breach-case-study/)
- [CodeCov Bash Uploader Compromise - 2021](https://www.securityweek.com/codecov-bash-uploader-dev-tool-compromised-supply-chain-hack/)
- [Kasya VSA ransomware (REvil) - 2021](https://cybernews.com/security/kaseya-ransomware-attack-heres-what-you-need-to-know/)
- [Log4Shell - 2021](https://en.wikipedia.org/wiki/Log4Shell)
- [3CX attack (SmoothOperator) - 2023](https://www.bleepingcomputer.com/news/security/hackers-compromise-3cx-desktop-app-in-a-supply-chain-attack/)
- [XZ Utils backdoor - 2024](https://en.wikipedia.org/wiki/XZ_Utils_backdoor)
- [npm debug and chalk compromise - 2025](https://www.aikido.dev/blog/npm-debug-and-chalk-packages-compromised)
- [LiteLLM PyPI backdoor - 2026](https://www.bleepingcomputer.com/news/security/popular-litellm-pypi-package-compromised-in-teampcp-supply-chain-attack/)
- [Axios infection - 2026](https://www.bleepingcomputer.com/news/security/hackers-compromise-axios-npm-package-to-drop-cross-platform-malware/)
- [SAP npm packages compromise - 2026](https://www.bleepingcomputer.com/news/security/official-sap-npm-packages-compromised-to-steal-credentials/)
- [TanStack compromise - 2026](https://www.bleepingcomputer.com/news/security/shai-hulud-attack-ships-signed-malicious-tanstack-mistral-npm-packages/)

[OPEN SOURCE MALWARE - Community Threat
Database](https://opensourcemalware.com/) is a community effort to track
malware spreading through open source packages.

Many of the recent examples listed above are linked to the same group of threat
actors known as TeamPCP.
They are exploited vulnerabilities in how the open source ecosystem commonly
have been operating.

## Security controls

Security controls are measures or safeguards that can be used to avoid or
minimize attacks.
We have divided these into two categories.
Some of them will be explored in further detail.

### Development

1. Provide security training
   - Provide adequate security training to developers
   - Foster a security focused company culture
2. [Threat Modeling](https://owasp.org/www-community/Threat_Modeling_Process)
3. Use Version control system (VCS)
   - Git has become the de facto
   - Use branch protection
   - Require code review before merging pull-requests
4. [Secure development environment](dev-env.md)
5. Dedicated build system
   - Use a dedicated build system separate from developer machines.
   - Sign builds.
   - Enforce only source code and signing keys as input to build system.
   - Use ephemeral build environment where possible.
6. Dependencies scanning (SCA)
7. Security testing (SAST, DAST, PenTest)

### Operation

1. Enforce network segmentation
2. Service isolation with VMs or containers
3. Web Application Firewall
4. Secret management - HashiCorp Vault, AWS Secrets Manager etc.
5. SIEM, XDR and SOAR
6. Infrastructure as Code (IaC) - Terraform, CloudFormation etc

## Maturing supply chain security

To further mature the security guarantees of your software supply chain, we like
to point you towards [SLSA](https://slsa.dev/) project.
It is a specification that can be followed for a milestone based of security
best practices in software supply chain.
It is made up of a source and a build track.
Each track is divided into several levels.
Where higher levels provides increased guarantees.
