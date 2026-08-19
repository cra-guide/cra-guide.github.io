+++
title = "Risk assessment"
weight = 5
+++

## Introduction

Manufactures must perform a risk assessment as described in [Article
13](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847#art_13)
paragraph 3-4.
The risk assessment should take into account the cybersecurity requirements in
[Annex
I](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847#anx_I).
Pay attention to part I, point (1) and (2).
In short, a risk assessment is required as part of the technical documentation.
It must document that the product has been produced with a reasonable level of
cybersecurity based on risk.
If any of the requirements in Annex I have been deemed not applicable, then it
must document the reasoning leading to the conclusion.

Note that other legislation (such as [NIS 2
](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng)) might impose additional
requirements to risk assessment for some product types.

## Risk assessment primer

Risk is the possibility that something unwanted or harmful might happen.

In general, for any risk, one might take one of 3 actions:

- Avoid
- Reduce
- Accept

Say you have identified a risk, that personal data could be leaked.
The risk can be **avoided** by not processing any personal data.
The risk can be **reduced** by encrypting the data.
The risk can be **accepted** if it is low enough.
Legislation like CRA and GDPR makes it prohibitively expensive to **accept** a
risk without lowering it to an appropriate level, taking the users assets into
consideration.

In order to talk about managing risks, it is useful to understand a bit of
terminology.

- **Asset** is a physical or digital resource that holds value to an
  organization or person.
- **Threat** is something that can compromise an asset.
  Whether intentional or accidentally.
  Examples: hacking, ransomware, power outage.
- **Compromise** is an occurrence that damages either integrity,
  confidentiality or availability of a system.
- **Vulnerability** is a weakness that can lead to such compromise.
  Examples: lack of backup, misconfigured firewall, employee falling for phishing.
- **Risk** is the probability of a threat exploiting a vulnerability.

You might say that _Risk = Threat × Vulnerability_

If we can guesstimate the impact (often measured in €), then we might change
the formula to:

_Risk = Threat × Vulnerability × Impact_

The risk assessment must be tailored to the product and should take into
consideration:

- How it interfaces with the world around?
  - Is it connected to the internet?
  - Can it cause damage to people and objects?
- What assets (on users end) can be impacted by a compromise and what loss can
follow?
- How attractive is it as a target?
  - Who is using the product? Consumer, critical infrastructure etc.

[CVSS](https://www.first.org/cvss/) scores can help determine which
vulnerabilities to focus on.

[Security testing](./supply-chain/security-testing.md) can be used to verify
and provide evidence that mitigations are effective.

## Threat modeling

Threat modeling should be performed as part of the risk assessment.
It is a structured approach to discover threats to a system. Various techniques
can be used, such as
[STRIDE](<https://learn.microsoft.com/en-us/previous-versions/commerce-server/ee823878(v=cs.20)>)
and [Attack
Trees](https://www.schneier.com/academic/archives/1999/12/attack_trees.html).

Some good resources to get started with threat modeling are:

- [Microsoft Learn - Threat Modeling Security Fundamentals](https://learn.microsoft.com/en-us/training/paths/tm-threat-modeling-fundamentals/)
- [OWASP - Threat Modeling Process](https://owasp.org/www-community/Threat_Modeling_Process)

## Reassessment

The risk assessment must be updated throughout the products support period
(expected lifespan).

The threat landscape is continuously evolving.
It is therefore recommended to stay up to date, by for instance reading the
latest yearly [ENISA - Threat
landscape](https://www.enisa.europa.eu/publications?search_api_fulltext=Threat+landscape)
report, and cybersecurity news sites such as
[BleepingComputer](https://www.bleepingcomputer.com/).

Changes in functionality can also prompt reassessment.
For instance when adding new capabilities to a product through a software
update.

Employ monitoring to detect new threats and update assessment accordingly.
Monitoring should be implemented in such a way as to respect users privacy.

## Risk assessment standards

A harmonized standard that covers ensuring an appropriate level of
cybersecurity based on the risks should be ready for adoption 30 August 2026.
In the meantime, manufactures might look towards existing recognized standards
such as ISO 27005, IEC 62443
([source](https://www.enisa.europa.eu/sites/default/files/2024-11/Cyber%20Resilience%20Act%20Requirements%20Standards%20Mapping%20-%20final_with_identifiers_0.pdf))
and NIST SP 800-30 ([source](https://www.secure4sme.eu/document/open?id=10)).

We also recommend reading "Risk Assessment 101" in [SECURE - The CRA’s
Essential Cybersecurity Requirements: Annex I, Part
I](https://www.secure4sme.eu/central-repository/the-cras-essential-cybersecurity-requirements-annex-i-part-i).
