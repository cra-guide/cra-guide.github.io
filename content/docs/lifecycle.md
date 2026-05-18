+++
title = "Lifecycle"
weight = 10
+++

This section is based on the stage that are traditionally included in the
software development life cycle.
The ideas are to gradually introduce concepts in a way that can be used as
reference along the way as your software development project moves along.

## Planning

Determine what [category](./product-categories.md) the product falls into.

Make sure developers are adequately trained.
Example [OpenSSF - Developing Secure Software
(LFD121)](https://openssf.org/training/courses/) course.

Conduct initial [risk assessment](./risk-assessment.md).

## Design

Manufactures must insure an appropriate level of security, taking well
established security principles into account such as: [secure by
design](https://en.wikipedia.org/wiki/Secure_by_design), [secure by
default](https://top10proactive.owasp.org/the-top-10/c5-secure-by-default/),
minimize [attack
surface](https://cheatsheetseries.owasp.org/cheatsheets/Attack_Surface_Analysis_Cheat_Sheet.html).

Regarding the data being processed and stored, the principle of [data
minimization](https://en.wikipedia.org/wiki/Data_minimization) must apply.
Also, [confidentiality, integrity and
availability](https://en.wikipedia.org/wiki/Information_security#CIA_triad) of
the data must be protected.

Threat modeling should be performed as part of this stage.
See [threat modeling](./risk-assessment.md#threat-modeling).

## Development

#### Secure coding practices

Developers should be familiar with and follow secure coding practices.
Some resources are: [OWASP Developer Guide](https://devguide.owasp.org/),
[OWASP
ASVS](https://owasp.org/www-project-application-security-verification-standard/),
[NIST SSDF](https://csrc.nist.gov/Projects/ssdf).

#### Make inventory of approved libraries

According to CRA, manufactures need to exercise due diligence when it comes to
3rd party components in software.
Maintaining an inventory of vetted frameworks, libraries and algorithms can
greatly assist in this regard.

#### Security testing

Regularly running automated security testing tools can help catch issues early,
without requiring significant manual work.
Such tools include:

- Software Component Analysis (SCA)
- Static Application Security Testing (SAST)
- Dynamic Application Security Testing (DAST)

A list of some tools/vendors can be found
[here](https://owasp.org/www-community/Source_Code_Analysis_Tools) and
[here](https://owasp.org/www-community/Vulnerability_Scanning_Tools).

Though such tools can help discover vulnerabilities, they also tend to report
false positives.
They also exhibit limited ability in discovering certain classes of
vulnerabilities, such as flaws in program logic.
It should be augmented with manual testing should be done, such as code review
and penetration testing.

## Testing

Verify that appropriate mitigations and security controls have been implemented
base on the risk assessment.

Verify that each of the security requirements in [Annex
I](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847#anx_I)
have been met.
Document how they have been met.

## Production

*For physical products.*

Ensure security of supply chain for hardware.
Take reasonable actions to ensure that components have not been tampered with.

## Deployment

#### When first put on the market

There are some procedures that need to take place when product is first put on
market, or if significantly altered.
Here is a quick overview, leaving out a lot of detail for brevity.

- Conduct conformity assessment.
- Draw up technical documentation.
- Affix CE marking.
- Draw up EU declaration of conformity.

See [Annex VIII Conformity Assessment
Procedures](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A02024R2847-20241120#anx_VIII)
for details.

Technical documentation and EU declaration of conformity must be kept for at
least 10 years or the support period, whichever is longer.

#### Before each release

Whenever new software is released, including security patches and functionality
updates.
Also when first put to market.

Generate and store SBOM.

Ensure there is no known exploitable vulnerabilities, including 3rd party
components.
SBOM and SCA are vital to ensure this requirement is met.

Make sure the digital elements have not been altered in such a way, as to make
the technical documentation invalid.

Ensure default configuration is secure.
Example: can not ship with a hard-coded default password.

## Maintenance

Scan SBOMs against databases of known vulnerabilities, such as [CISA
KEV](https://www.cisa.gov/known-exploited-vulnerabilities-catalog),
[CVE](https://www.cve.org/), [NVD](https://nvd.nist.gov/),
[EUVD](https://euvd.enisa.europa.eu/), [VulDB](https://vuldb.com/),
[Snyk](https://security.snyk.io/) etc.

Provide OTA (over-the-air) updates (if applicable) with security fixes
throughout the support period.

Monitor for new threats.

## End-of-life

TODO

## Methodologies

It can be an idea to look into methodologies or process models that incorporate
security as an integral part of software development.
Example: [Microsoft
SDL](https://www.microsoft.com/en-us/securityengineering/sdl/practices) and
[NIST SSDF](https://csrc.nist.gov/Projects/ssdf).
