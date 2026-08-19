+++
title = "Security Testing"
+++

Security testing is an important for ensuring the product can live up to the
security requirements in [ANNEX I](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847#anx_I).
Security testing and reviews are required according to ANNEX I, Part II, (8).
Test results assist in demonstrating compliance during [conformity assessment](../conformity-assessment.md).

This page lays out a broad outline on security testing.

# Software Composition Analysis (SCA)

Most modern software is build on open source components (dependencies).
To fulfill the requirement, that products shall be made available on the market
without known exploitable vulnerabilities (ANNEX I, Part I 2.a), requires
awareness of vulnerabilities in open source components.
SCA can detect if dependencies are up-to-date, contain known vulnerabilities
and in license compliance.
It can not determine whether the vulnerability is exploitable through your
product.
SCA tools can work with package lock files or [SBOMs](./sbom.md).
You can think of SCA as `npm audit`, but working across different package
managers and build systems.

We recommend scanning dependencies with an SCA tool such as [OWASP
dependency-check](https://github.com/dependency-check/DependencyCheck) or
[Syft](https://github.com/anchore/syft)+[Grype](https://github.com/anchore/grype)
on a regular basis.
It can be automated in CI/CD by adding it as a check when merging into release
branch.

# Static application security testing (SAST)

SAST tools can detect vulnerabilities in source code through static analysis.
Traditional SAST used a combination of pattern, rules and analysis of abstract
syntax tree (AST).
Many modern SAST tools also utilizing LLMs to detect issues that are hard to
write patterns for and to better explain findings.

SAST can be executed from CLI, build into CI/CD and integrated into the IDE
through plugins.
IDE integration provides real-time feedback as the code is being written.

SAST is good at detecting potential issues early in the software development
life cycle (SDLC), where they are cheaper to fix.
It is better at detecting some categories of vulnerabilities than others.
Also, SAST commonly report many false positives which can contribute to
developer fatigue.

For a list of tools, see [Source Code Analysis
Tools](https://owasp.org/www-community/Source_Code_Analysis_Tools).

# Dynamic Application Security Testing (DAST)

DAST tools can detect potential vulnerabilities in running applications.
This category of security testing tools are commonly used for web applications.
DAST is a good supplement SAST.
They allow for more frequent testing than manual penetration testing.

We recommend running a SAST tool on release to staging/pre-production.

For a list of tools, see [Vulnerability Scanning Tools](https://owasp.org/www-community/Vulnerability_Scanning_Tools).

# Vulnerability scanners

Vulnerability scanners are automated tools aimed at detecting vulnerabilities
on (machine) host level.
Periodic scanning of servers and devices can help detect vulnerabilities and
expose attack surface visibility before exploitation.

If for example you develop products based on embedded Linux, it can be an idea
to run such a scanner against the device on regular interval (monthly,
quarterly or yearly).

Well known examples of such tools are [Tenable
Nessus](https://www.tenable.com/products/nessus) and [OPENVAS by
Greenbone](https://openvas.org/).

# Secret scanning

Secrets such as default passwords, API tokens, encryption keys etc. often end
up source repositories and build artifacts such as binaries or container
images.
A category of tools commonly known as secret scanners can help avoid accidental
disclosure of secrets.
These tools can also assist in fulfilling the requirement of secure by default
configuration (ANNEX I, Part I 2.b), by detecting default password,
certificates etc.
It is important to note that these scanners are based on patterns and rules,
and therefore provides no guarantee that a secret will be detected.
Such tools should be used in combination with rigors manual review.

# Penetration testing

Penetration testing (pentest for short) is the most realistic form of security
testing, as it aims to attack the product the same way as a real threat actor.

A pentest is conducted by a skilled professional.
Often as a consultant from a specialized pentest company.
Even if two pentesters follow same methodology, the result will depend on the
skill and creativity of the individual penters.
A pentest can only show the presence of vulnerabilities, not the absence.

Conducting a pentest can be expansive compared to more automated forms of
security testing.
It is therefore not feasible for most SMEs to conduct a pentest on their
product very often.

A pentest can be used increase assurance that a product lives up to security
requirements before being put on market, and with major modification to design
of existing product.
Whether a pentest is necessary depends on the risk profile of product
([category](../product-categories.md) and [risk
assessment](../risk-assessment.md)).
It is best used in addition to other kinds of security testing, as a pentest
only makes sense late in the development life cycle.
Please beware that it is important that enough time is given to the pentester
and fixing potential issues, before launching the product.

# Word of caution

Running any form of security testing against a live production OT environment
comes with a significant risk.
The unusual traffic patterns such testing generates can crash devices affecting
available of the production line.
Or worse, resulting in misbehavior violating safety, thereby causing risk of
damage to material or personal.

# Vendors and products

Many security testing tools have capabilities that span several of the
categories discussed on this page.
A none exhaustive list of products/vendors for the categories of tools
discussed in this page are:

- [Snyk](https://snyk.io/)
- [Veracode](https://www.veracode.com/)
- [Semgrep](https://semgrep.dev/)
- [PortSwigger](https://portswigger.net/)
- [Rapid7](https://www.rapid7.com/)
- [Aikido](https://www.aikido.dev/)
- [Tenable](https://www.tenable.com/)
- [Black Duck](https://www.blackduck.com/)
- [Mend.io](https://www.mend.io/)
- [JFrog](https://jfrog.com/)
- [Sonatype](https://www.sonatype.com/)
- [Checkmarx](https://checkmarx.com/)
- [Cycode](https://cycode.com/)

*We do not have affiliation with any of these vendors.
Nor do we recommend a particular vendor over another.
Do your own research.*

<!-- > It must be highlighted that cybersecurity testing is not deterministic as in -->
<!-- > other NLF-regulated fields and the results might not be unique. -->
<!---->
<!-- Source: [FAQs on the Cyber Resilience Act, 6.5](https://ec.europa.eu/newsroom/dae/redirection/document/122331). -->
