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

**Secure coding practices**

Developers should be familiar with and follow secure coding practices.
Some resources are: [OWASP Developer Guide](https://devguide.owasp.org/),
[OWASP
ASVS](https://owasp.org/www-project-application-security-verification-standard/),
[NIST SSDF](https://csrc.nist.gov/Projects/ssdf).

**Make inventory of approved libraries**

According to CRA, manufactures need to exercise due diligence when it comes to
3rd party components in software.
Maintaining an inventory of vetted frameworks, libraries and algorithms can
greatly assist in this regard.

**Security testing**

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
They also have limited ability in discovering certain classes of
vulnerabilities such as flaws in program logic.
In addition, manual testing should be done, such as code review and penetration
testing.

## Testing

Verify that appropriate mitigations and security controls have been implemented
base on the risk assessment.

Verify that each of the security requirements in [Annex
II](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202402847#anx_II)
have been met.
Document how they have been met.

## Additional resources

- [Microsoft SDL](https://www.microsoft.com/en-us/securityengineering/sdl/practices)
