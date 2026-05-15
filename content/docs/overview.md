+++
title = "Overview"
weight = 1
+++

# Introduction

Cyber Resilience Act also known as CRA entered into force 10 December 2024.
It will be fully applicable by 11 December 2027.
It encompasses all "products with digital elements" placed on the European
market.
Where "product with digital elements" should be interpreted as all products
that include software in any way.
Whether the product it is software or hardware.
Something that is purely <abbr title="Software as a Service">SaaS</abbr> is
excluded.
Because the legislation is about products, not services.
Something that is monetized as SaaS, but have a software component to it that
runs on something that is in the consumers possession is still in scope.
So software installed on an electronic device (including computer) within the
consumers possession is in scope.
Software that is only accessed through a web-browser will likely be exempted.

# Timeline

The rollout of CRA happens in stages.
Here is quick overview

- Entry into force: 10 December 2024
- Vulnerability reporting obligations: 11 September 2026
- Fully applies: 11 December 2027
  - Products with digital elements put on EU market after this day must be
  fully compliant.

# Area of focus

The legislation requires obligations from various entities.
Including:

- Manufactures
- Importers
- Distributors
- Open Source stewards

On this site we focus solely on the obligations involving manufactures.
Manufacture is defined as:

> A natural or legal person who develops or manufactures products with digital
> elements or has products with digital elements designed, developed or
> manufactured, and markets them under its name or trademark, whether for
> payment, monetisation or free of charge.

See [Article 13 - Obligations of
manufacturers](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847#art_13)

# Compliance roadmap

This section aims to give a quick overview of what needs to be done to be
compliant with CRA.

1. Identify what products have digital elements (software)
2. Determine what category each product falls into (see
   [categories](./product-categories.md)), because that determines how assessment
should be done.
3. Conduct a gap analysis (see [gap analysis](./gap-analysis.md))
    - What standards are already implemented, and what are the gap between that
    and the obligations in CRA.
4. Make a risk assessment of the product (see [risk assessment](./risk-assessment.md))
    - Must account for how the product will likely be put to use.
    - Must consider damages to user assets if Vulnerability in the product is
    exploited.
    - Should include threat modeling.
    - Must include how security requirements of Annex I, part 1 is implemented,
    or reason why a requirement is not applicable.
    - Must include the application of security by design principle to archive
    an appropriate level of cybersecurity base on risk.
5. During development
    - Consider security as integral part of design.
    - Implement appropriate security controls.
    - Conduct regular security testing (see [security testing](./supply-chain/security-testing.md))
    - Configure CI/CD pipeline to generate SBOM (see [SBOM](./supply-chain/sbom.md)).
    - Daily scan SBOM of all released versions against databases of known
    vulnerabilities such as [CISA
    KEV](https://www.cisa.gov/known-exploited-vulnerabilities-catalog),
    [CVE](https://www.cve.org/), [NVD](https://nvd.nist.gov/),
    [EUVD](https://euvd.enisa.europa.eu/), [VulDB](https://vuldb.com/),
    [Snyk](https://security.snyk.io/) etc
    - Prepare to deliver security patches separate from new features.
6. Before release to market
    - Determine support period
      - Should be however long the product is reasonably expected to last (at
      least 5 years).
      - Must be documented and available at time of purchase.
    - Setup procedures for vulnerability management, including reporting to
    notified body.
    - Compile technical documentation (see [ANNEX VII](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847#anx_VII).
      - Details of manufacture.
      - Details of product.
      - How to use securely.
      - Single point of contact for reporting vulnerabilities
    - Conduct compliance assessment.
    - Draw up a declaration of conformity (see [Annex
    V](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847#anx_V)
    & [Annex
    VI](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847#anx_VI)).
    - Affix CE marking to the product.

*The text above is meant to give an overview of some key points to consider.
It is no substitute for reading obligations in CRA text.*

# CRA and Open Source

Early proposals of the legislation was heavy criticized by the free and open
source community.
Which lead to free and open source mostly being exempted from obligations in
the final legislation.
However, commercial activity build on <abbr title="free and open source
  software">FOSS</abbr> is in scope.
It means your hobby project on GitHub is not a concern.
But if sell products or services around FOSS then you have obligations.
Open source developer should sough out guidance regarding CRA elsewhere,
because as said, we solely focus on manufactures here.

If you are a manufacture and include open source components in your products,
then you become responsible for baseline security of those components.

The CRA legislation suggest the development of harmonized standards which can
streamline and unify manufacture conformity.
However, as of writing, such harmonized standards haven't been published yet.
