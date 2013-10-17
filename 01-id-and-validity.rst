Identity and validity
=====================

Concepts
++++++++

**Identity** (a user). The actual underlying sovereign user, an entity external to the identity system capable of interacting with it and initiating action. [#M0]_ By default, security models tend to assume that each identity acts **independently** [#M0b]_; there are security consequences if this assumption is broken.

**Identifier** (a piece of data). One must be able to uniquely point to an identity. In PGP this is done using a *key*; in web social networks this is done using a *user profile id*. Typically, jargon terms involving "id" stand for "identifier" and not the underlying identity. In both cases it is quite easy to create invalid (fake) identifiers. [#M2]_

**Validity** (a security property). One must know that the identifier actually is controlled by (writeable by, useable by) the identity. Validity itself has only two states - "valid" and "invalid". However, we might not have perfect knowledge, so we also need a third state, "unknown". [#M1]_ [#F1]_

**Verification** (a process). This is a process that you run to check the state of some security property. In the context of identity systems, unqualified "verification" usually refers to verification of identifier validity. Other forms of verification exist (e.g. cryptographic signature verification), so it's important to acknowledge this implicit convention.

**Verification of validity**. Validity cannot be objectively calculated; a source identity must check a target identity. In many systems this is an ad-hoc process and that works well. On web social networks you might look at the behaviour of the account and check that it matches the character of the person. Of course, this is not fully secure, but "good enough" for everyday purposes in that environment.

The strength of a chain is only as good as its weakest link though, so if we wish to have higher confidence in our security, we need much stricter verification processes, whose strength can match the strength of the cryptographic parts of the system. Otherwise, the latter can simply be bypassed by holes in the former, and is rendered pointless (and even a waste of resources to have developed).

Full verification
+++++++++++++++++

**Full verification**. The (idealised) process that gives us the theoretical maximum confidence. Generally, the source requests an **authentic confirmation** *C* from the target identity that they control the identifier. If there is reason/incentive for the actual real target identity to lie about this (if the identifier might belong to someone else), the source should also request some **unforgeable proof** *P* of this control. More precise discussion is delegated to the footnote [#M5]_ - also note that the process might be *structurally* different. [#M6]_

In a web social network, *C* might be to manually ask the target/friend via another channel (e.g. IM), and *P* might be to ask the friend to send a message from that profile, to prove they have access to it. Normally you wouldn't even bother with *P*, unless your friend is trying to convince you that they control some other unexpected profile.

Traditionally in cryptosystems, *C* is achieved by receiving the target's key fingerprint, and *P* is achieved by asking them to decrypt something using that key. [#M6]_ More recent developments are slightly more convenient - such as using the `Socialist Millionaire <http://en.wikipedia.org/wiki/Socialist_millionaire>`_ protocol to confirm a known shared secret. However, they all still require a **known secure channel** for an initial transfer of authentic data (the fingerprint or the shared secret). In many cases this is a physical meeting, but you could also use a channel to a different identifier on a different medium, as long as it is known-valid for the target - e.g., receiving your target's OTR fingerprints via email, signed by a PGP key known-valid for the target.

Verification in PGP
-------------------

(You can skip to the next section if you don't care about PGP.)

You might have noticed that the above outline only verifies the key against the identity. PGP however, also attaches other metadata to the key, called UIDs [#F2]_, typically containing the name and email address of the person. In a simple system, these UIDs would be treated as user-produced data, with no security semantics. If you don't verify this data separately, then you must have confidence in the identity (i.e. their integrity and competence) in order to have confidence in that data. [#F3]_ PGP does things differently, which gives rise to several issues.

In PGP, you can record [#M7]_ that you verified the UIDs, which is powerful and gives extra confidence in secure email. On the other hand, verifying UIDs is much less reliable than verifying control of a key [#F4]_, so one argument is that the security gain doesn't justify the additional complexity cost. But opting-out is not an option; PGP *forces* you to verify UIDs - you cannot record directly the fact that you verified the key, you must instead record that you verified *some/all UIDs*, from which PGP infers that you also verified the key. This is very difficult to get right, and even more difficult to explain how and why you should do this to a newcomer.

PGP combines the name and email address into an atomic UID, but these are semantically distinct identifiers on different mediums. I could lie about one and not the other - then the correct thing would be to verify the UID as INVALID. But many people get confused by this, and only verify my name during a physical meeting, but don't verify that I also control the email address. (`caff` does get this right.)

Typical PGP software implicitly conflates the UID identifier with the identity itself, and only mention "the key validity" and not "the UID validity". This is an unrealistic simplification of the security model (granted, a complex one) and does not help the UX to describe the security situation accurately.

Partial verification
++++++++++++++++++++

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

.. [#M0] This might not be a real living thing - it might be one of many virtual alias identities of a single living thing, or it might be a group virtual identity that represents many living things. TODO: talk about the detection of the real equivalence of multiple identities, how this might be used to defend against the sybil attack, but also to break the pseudonymity of a refugee.
.. [#M0b] A better way of saying this might be "able to express self-interest" - two identities might co-operate for mutual benefit, and the security model would still hold. However, if one identity is totally controlled by another, such that the actions of the slave are only really *extensions* of the actions of the master, and this is not expressed in the model, then the security properties achieved by the model may be broken.
.. [#M1] A more precise encoding could be 3 probabilities that sum to 1, to represent our beliefs in the state being "valid", "invalid", or "unknown" (our uncertainty).
.. [#M2] This low cost forms the basis of many attacks on the system.
.. [#M3] Perhaps with a way to sychronise it between devices, but we can ignore that extension for now.
.. [#M4] There are some issue with this, which we'll talk about in the later sections. TODO
.. [#M5] Authentic means that it is absolutely certain the information came from the target as intended, and was not altered during transit. Unforgeable means that it is absolutely certain that the target could not construct a fake proof. "Absolute" might be interpreted a little more loosely in the real world, but even in low security settings this must never include "trust in a third party", because that lowers it much too severely. Instead, third parties ought to be properly modelled by the theory of the security model.
.. [#M6] Certain tools structure this process differently, but the outcome is the same. For example, `caff`, a verifier tool for PGP, signs the key (with the verified fingerprint, *C*) unconditionally, then encrypts the signed key to the identifier - so that only someone who really controls the identifier can obtain the signature and make use of it, thereby *implicitly* achieving *P*.
.. [#M7] Verification without being able to record it to inform your software, is useless for the software.

.. [#F1] So already we see a flaw of the PGP model - it does not let users represent "verified as invalid". But this is fixable on the implementation/UI side.
.. [#F2] The unfortunately-named "UID" is actually a name; "u" stands for "user" not "unique".
.. [#F3] For example, even if you verified the validity of a web social network profile, your friend might be lying that they own `john@whitehouse.gov` - you would need to verify this latter fact separately.
.. [#F4] Verifying an email address is much more unreliable than verifying control of a key - they could control the key but not the email address, but be able to subvert the email system to send/receive messages to/from that address, even with messages signed/encrypted with their key. Verifying a name might also be problematic if you don't know the person - sometimes people ask for government-issued ID, but this is quite easily forgeable, especially to layman who don't know the anti-forgery characteristics a real ID should have. (It is also inappropriate for someone that wants to be known via a pseudonym.)

.. [#O1] People that say "SSH/OTR succeeded where PGP failed" don't understand the problem - we want to get as close to full verification as possible. Both SSH/OTR and PGP can be improved in this area, the difference is that the PGP model is *capable* of being improved, whereas SSH/OTR has no identity management primitives whatsoever.
.. [#O2] HTTPS and anything that uses X509 is only partially secure because you assume trust in the root CAs.
.. [#O3] Some systems claim they solve this; they are wrong. TODO: expand

