Identity and validity
=====================

Concepts
++++++++

**Identity** (a user). The actual underlying sovereign user, able to *do things*.

**Identifier** (a piece of data). One must be able to uniquely point to an identity. In PGP this is done using a *key* with identifying metadata (name, email, url) called UIDs [#F1]_; in web social networks this is done using a *user profile*. Typically, jargon terms involving "id" stand for "identifier" and not the underlying identity.

**Validity** (a security property). One must know that the identifier actually is controlled by (writeable by, useable by) the identity. Validity itself has only two states - "valid" and "invalid". However, we might not have perfect knowledge, so we also need a third state, "unknown". [#M1]_ [#F2]_

In PGP, this would be if the key is actually controlled by its UIDs. On a web social network, this would be if the user profile is actually controlled by the claimed name. In both cases it is quite easy to create invalid (fake) identities. [#M2]_

**Verification** (a process). This is a process that you run to check the state of some security property. In the context identity systems, unqualified "verification" usually refers to verification of identifier validity. Other forms of verification exist (e.g. cryptographic signature verification), so it's important to understand this implicit assumption in terminology.

**Verification of validity**. Validity cannot be objectively calculated; a source identity must check a target identity. In many systems this is an ad-hoc process and that works well. For example, on web social networks you might look at the behaviour of the account and check that it matches the character of the person. In systems that provide cryptographic levels of security, we need much stricter processes to match the strong cryptographic security the in other parts of the system. Otherwise, that strong security is pointless.

We'll call the best possible process **full verification** - the process is done by yourself, and it achieves full certainty as to the result. Generally, the source requests an unforgeable proof from the target identity that they control the identifier. The traditional method in cryptosystems, is to establish the target's key fingerprint via a known secure channel (e.g. physical meeting), then ask them to decrypt something using that key [#F3]_. More recent developments are a bit more convenient, e.g. using the socialist millionaire protocol. In a web social network, one might ask the target to send a message from that profile, to prove access.

Verification of validity
++++++++++++++++++++++++

Verification is a logistic activity (*not* cryptographic) and carries a cost. Full verification is the most costly; and it is considered infeasible for everyone to fully verify everyone else. We would prefer not to sacrifice security, so we devise systems to improve this situation. These *all* sacrifice full verification for lower cost [#O3]_, and can be categorised as follows:

.. _reduce-verification-cost:

1. negligible-cost automatic partial verification (e.g. trust-on-first-use aka TOFU, as per SSH/OTR [#O1]_)
2. low-cost manual partial verification (e.g. semi-secure key distribution via HTTPS [#O2]_, or "download this from many different locations")
3. transitive partial verification over pre-existing valid channels (e.g. X509 hierarchy, PGP's web-of-trust)

1 and 2 lower the cost of each verification, whereas 3 lowers the global cost of verification, by reducing the total number needed.

GPG does not implement (1) or (2), and has a very ad-hoc implementation of (3) that no-one uses. Effectively, people are forced into full verification even when they don't want to expend this cost. Other systems have weaker security guarantees (such as TLS/X509 and SSH/TOFU) but are more convenient to use, and consequently are more widespread.

But we can (and must) use all of these strategies at the same time, to compete successfully against low-cost, lower-security solutions. We must aim for widespread adoption in order to massively raise the cost of population-scale attacks. Doing this will also expose users to identity management concepts and terminology, which provides a smoother training path for even non-technical people to eventually understand how and when to perform full verification.

Automatic partial verification
------------------------------

TOFU requires client support, but this should be straightforward if not simple - e.g. Enigmail can increment a counter for every new email received. To avoid unnecessarily updating the public key, this data should only be available to the local client [#M3]_.

A user interface might display the following information:

- since 2013-01-01 you interacted with this key 16 times over email, 16 of which were from matching email addresses
- since 2013-01-02 you interacted with this key 10 times over 3 public keyservers
- since 2013-01-03 you interacted with this key 23 times over SSH (via a monkeysphere extension)

This area needs further work!

Manual partial verification
---------------------------

Partial manual verification requires a way to represent this. PGP allows one to add custom notations to certifications [#M4]_, colloquially "key signatures".

One principle to maintain is that canonical data must only describe objective facts, and not encode subjective assertions about what those facts cause us to believe. Instead, the latter should be inferred by the local client application and cached rather than published.

For example, some facts we might want to encode, that could affect our belief in the key's validity, for a particular verification attempt:

- transmission method (e.g. via phone, https, physical?)
- the format of the data (e.g. hex code fingerprint, QR code, photograph?)
- proof method (e.g. asked target to decrypt, asked target to sign, assumed target was trustworthy?)

A user-interface might display the following information:

- on 2013-01-01 you verified this key as valid using "https://url; recorded by enigmail"
- on 2013-01-02 you verified this key as valid using "physical; recorded by caff/offline" # this is actually full verification; here as an example for illustration
- on 2013-01-03 you verified this key as INVALID using "phone-landline; recorded by enigmail"

This area needs further work!

Gamification
------------

Having a taxonomy of verification methods opens up the possibility to gamify this process, which might encourage people to do it further. It suggests simple meter-based interfaces that aren't treated too seriously by the user, avoiding false expectations on security. Of course, the objective facts should always be available in the interface.

This area needs further work!

UX workflows and UI mockup
--------------------------

TODO(infinity0): Do that diagram in SVG.


----

.. [#M1] A more precise encoding could be 3 probabilities that sum to 1, to represent our beliefs in the state being "valid", "invalid", or "unknown" (our uncertainty).
.. [#M2] This low cost forms the basis of many attacks on the system.
.. [#M3] Perhaps with a method to sychronise it between devices, but we can ignore that extension for now.
.. [#M4] There are some issue with this, which we'll talk about in the later sections. TODO

.. [#F1] The unfortunately-named "UID" is actually a name; "u" stands for "user" not "unique".
.. [#F2] So already we see a flaw of the PGP model - it does not let users represent "verified as invalid". But this is fixable on the implementation/UI side.
.. [#F3] In many cases for PGP this is done incorrectly - only the key fingerprint is checked, and you trust that the target controls that key. The web social network analogy would be simply believing the target when they say they own that profile. Then, your security depends on your target being truthful.

.. [#O1] People that say "SSH/OTR succeeded where PGP failed" don't understand the problem - we want to get as close to full verification as possible. Both SSH/OTR and PGP can be improved in this area, the difference is that the PGP model is *capable* of being improved, whereas SSH/OTR has no identity management primitives whatsoever.
.. [#O2] HTTPS and anything that uses X509 is only partially secure because you assume trust in the root CAs.
.. [#O3] Some systems claim they solve this; they are wrong. TODO: expand

