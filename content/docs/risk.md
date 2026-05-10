+++
title = "Risk assessment"
weight = 3
+++

# Introduction

Manufactures must perform a risk assessment as described in [Article
13](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847#art_13)
paragraph 3-4.
The risk assessment should take into account the cybersecurity requirements in
[Annex
I](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847#anx_I).
Pay attention to part I, point (1) and (2).
In short, a risk assessment is required as part of the technical documentation.
It must document that the product has been produces with a reasonable level of
cybersecurity according to the risks.
If any of the requirements in Annex I have been deemed not applicable, then it
must document the reasoning leading to the conclusion.

Note that other legislation (such as [NIS 2
](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng)) might impose additional
requirements to risk assessment for some product types.

# Risk assessment primer

Risk is the possibility that something unwanted or harmful might happen.

In general, for any risk, one might take one of 3 actions:

- Avoid
- Reduce
- Accept

Say you have identified a risk that personal data could be leaked.
The risk can be **avoided** by not processing any personal data.
The risk might be **reduced** by encrypting the data.
Legislation like CRA and GDPR makes it expensive to **accept** such risk.

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

# Threat modeling

Threat modeling should be performed as part of the risk assessment.
Threat modeling is a structured approach to discover threats to a system.
Various techniques can be used, such as
[STRIDE](<https://learn.microsoft.com/en-us/previous-versions/commerce-server/ee823878(v=cs.20)>)
and [Attack
Trees](https://www.schneier.com/academic/archives/1999/12/attack_trees.html).

Some good resources to get started with threat modeling are:

- [Microsoft Learn - Threat Modeling Security Fundamentals](https://learn.microsoft.com/en-us/training/paths/tm-threat-modeling-fundamentals/)
- [OWASP - Threat Modeling Process](https://owasp.org/www-community/Threat_Modeling_Process)

# Standards

CRA does not dictate any specific standard or methodology for the risk assessment.
Once a harmonized standard has been developed it will be what you should look
towards.
In the meantime, manufactures might look towards existing recognized standards
such as ISO 27005, IEC 62443
([source](https://www.enisa.europa.eu/sites/default/files/2024-11/Cyber%20Resilience%20Act%20Requirements%20Standards%20Mapping%20-%20final_with_identifiers_0.pdf))
and NIST SP 800-30 ([source](https://www.secure4sme.eu/document/open?id=10)).
