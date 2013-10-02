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

We develop a coherent and precise language to talk about identity management. We describe both PGP and non-cryptographic identity systems using this language. We reason about various security properties using this language, independently of the cryptography used to achieve said properties. We conclude that the UX need not resort to cryptographic language.

We describe some usability fails of existing implementations. We suggest UX-driven strategies for improving convenience and describe them terms of cost. The model and the UX are developed together. We show that some strategies are orthogonal (e.g. TOFU-vs-WoT) and so can be implemented together.

We describe trust and trust paths to scale the verification process. We talk about the problem of social graph mining. We introduce transient identifier systems, and discuss the tradeoffs with the traditional long-term identifier paradigm.

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

