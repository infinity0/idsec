Transient identifiers
=====================

We saw the problems with traditional long-term-identifier identity systems in the previous chapter. Some recent identity systems shift away from this paradigm.

In this chapter we will talk about the issues and tradeoffs between long-term vs transient identifiers.

Identity via introduction
-------------------------

A concrete example of a transient id system is one where you can only refer to an identity using a subjective identifier from a trusted introducer. Consequences of this:

* Much more costly to query and mine the social graph (if/until 0k-proof graph data structures are developed)
* You can only use one path to refer to someone.
* The social graph is a tree and you must deduplicate (i.e. check that two paths actually point to the same real identity) out-of-band
* Trust algorithms that can aggregate "trust scores" from many many long paths to the same destination, do not work here.

Identities do not generate long-term asymmetric keypairs, but only generate a symmetric shared secret with each introducer they need to communicate with. This has the additional feature:

* Requires short expiry rather than key update infrastructure. Secrets are re-generated every session.
  * But what if the shared secret is lost?

Example concrete systems: `Briar <http://briar.sourceforge.net/>`_
