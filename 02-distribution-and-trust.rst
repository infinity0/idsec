Metadata distribution and trust
===============================

Motivation. Recall :ref:`reducing verification cost <reduce-verification-cost>`. Strategies (1) and (2) only reduce the cost of each verification, so that the global cost complexity is still O(n^2). On the other hand, strategy (3) has the potential to reduce the cost complexity too. There are several sub-strategies for this:

- objective processes, one that is universal (e.g. mathematical) and correct
- pseudo-objective processes, constructing some architecture of other parties to perform the process, and assuming they are honest and correct
- subjective processes, that is dependant on yourself and what you believe about others. Note that full verification is a subjective process - you run the process from your environment and your beliefs.

Honor and trust
+++++++++++++++

**Honor** (a security property). This is the property that an identity behaves correctly (i.e. honestly and competently) within the protocols set out by the identity system. This would include, for example, answering questions, reporting information, executing verification processes against others, etc.

**Verification of honor** (a process). Similar to verification of validity, a source identity can increase their belief in the honor of a target identity. Typically this is a long social process which cannot possibily be automated, but we state the concept here to show that our language is consistent and precise.

**Trust** (a subjective belief). The extent to which a source has verified the honour of a target. [#M5]_ "To trust" as a verb means "to have verified the honour of".

Since we define "correct behaviour" of a target to include how they trust others, trust chains are a coherent concept. [#M6]_

Trust algorithms
----------------

Calculating overall transitive trust of a trust chain.

The problem of short chains vs long chains. Sybil attacks.

How to use them to implement near-global decentralised naming, or solving Zooko's triangle.

Key infrastructure
++++++++++++++++++

Key updates
-----------

Adding new signatures and revoking old signatures.

Ensuring freshness.

Contrast this with short-expiry schemes.

Name-is-key schemes
-------------------

Why they don't actually solve anything.

Doesn't support subkeys, key updates.

Don't get rid of security requirement to verify validity.

Hiding the social graph
-----------------------

Disadvantages of public keyservers.

Encrypting signatures to friends only.

f2f infrastructure support for distributing keys.

Cost of querying the graph.

----

.. [#M5] PGP documentation doesn't distinguish between honor vs subjective belief in honour, and doesn't mention verification. We have chosen to use different terminology to precisely distinguish between objective fact and subjective belief. Also the word "trust" has strong connotations of subjectivity, and isn't appropriate for describing a security property.
.. [#M6] By contrast, validity chains are not a coherent concept.
