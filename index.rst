.. idsec documentation master file, created by
   sphinx-quickstart on Wed Oct  2 10:15:46 2013.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Security topics in identity systems
===================================

Contents:

.. toctree::
   :maxdepth: 2
   :glob:

   *

This project arose out of discussions with engineers and UX designers during CTS IV, Berlin 2013.

**Motivation**. Identity - how it is represented, how they interact with each other, etc - is a fundamental aspect of any user-oriented security system. It is often overlooked because it is so implict in the design, and not directly to do with what the system is trying to achieve (e.g. secure communication or storage), that both users and developers neglect to give it proper consideration.

Since the subjects are "In Real Life", full security cannot be achieved purely through software. [#A1]_ However, many systems leave this "human gap" open very wide, with almost no explicit semantics on what the user should do to close it. [#A2]_ The UX corresponding to these parts tend to be complex or vague, so that only the most expert specialists will understand and use the tool correctly. This is similar to leaving half of a program undefined and hoping to get the correct answer out - no wonder the user leaves frustrated, and we don't achieve any security!

We ought to treat the end user as a dumb machine that can execute tasks, but not (initially) understand the security semantics of what they are doing. Moreover, they are unreliable, and so we must fail gracefully for the cases where a process is only half-completed or not done at all. As with a dumb machine, we can generally expect correct answers to questions (especially if it is in the user's interest), but only if those questions are simple and precise - not vague like "how carefully have you verified this key, choose from {1,2,3}?".

On top of this, many security systems use technical cryptographic language to describe their security features, but that is not directly what the user wants. (To borrow language from systems architecture) cryptography is the *implementation* and the security model is the *interface*. It is this latter abstract thing that the user cares about, and which must be presented in the user interface. [#A3]_

**Aims**. We develop a coherent and precise language to talk about identity management. We describe both PGP and non-cryptographic identity systems using this language. We reason about various security properties using this language, independently of the cryptography used to achieve said properties. We conclude that the UX need not resort to cryptographic language.

We describe some usability fails of existing implementations. We suggest strategies for closing the "human gap", and describe their convenience cost and their security effectiveness. The model and the UX are developed together. We show that some strategies are orthogonal (e.g. TOFU-vs-WoT) and so may be implemented together.

We describe trust and trust paths to scale the verification process. We talk about the problem of social graph mining. We introduce transient identifier systems, and discuss the tradeoffs with the traditional long-term identifier paradigm.

.. [#A1] This is explained and justified in more detail later.
.. [#A2] In PGP, users are expected to manually verify the validity of others' keys, and build up their own web-of-trust. But there are no explicit instructions on how this might be achieved.
.. [#A3] For example, OTR software typically uses "private" and "authenticated", rather than "encrypt" and "sign" (here just as an example; they are not direct counterparts).


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

