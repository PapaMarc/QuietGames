# Technical Brief: Alumni Email Identity Preservation via Exchange Online Routing‑Only Model

## Overview

Many leading universities—including Harvard, Stanford, MIT, the University of Pennsylvania, Princeton, Dartmouth, and Brown—preserve alumni email identity without hosting mailboxes or relying on user‑controlled forwarding. These institutions use a routing‑only model within Exchange Online that maintains the alumni domain (e.g., **@alumni.duke.edu**) while eliminating the operational and security burdens associated with lifetime mailboxes.

This model directly addresses the deliverability and forwarding‑related issues described in Duke’s recent communication while preserving the long‑term institutional value of alumni digital identity.

\---

## 1\. Mail‑Enabled User (MEU) Architecture

Alumni accounts are converted from full mailboxes into **mail‑enabled users (MEUs)**:

* No mailbox storage
* No authentication surface
* No MFA or credential lifecycle
* No forwarding rules
* Retains the alumni SMTP identity (e.g., **@alumni.duke.edu**)
* Uses the **ExternalEmailAddress** attribute to define the alum’s preferred destination

This eliminates dormant mailbox risk, reduces administrative overhead, and removes the forwarding mechanisms that trigger rejection by external providers.

\---

## 2\. Controlled Inbound Routing

Inbound mail to alumni addresses is routed to each alum’s preferred external email address using one of two supported Exchange Online mechanisms:

### A. Transport Rule Routing

A single transport rule:

* Matches mail addressed to the alumni domain
* Uses **RedirectMessageTo** to route mail to the MEU’s ExternalEmailAddress
* Applies Exchange Online spam filtering
* Provides centralized logging and diagnostics

### B. Dedicated Connector

A dedicated **Outbound to Partner** connector routes alumni mail externally:

* Ensures DKIM/DMARC/SPF alignment
* Improves deliverability consistency
* Avoids consumer‑provider forwarding rejection

Both approaches are widely used in higher‑education alumni programs.

\---

## 3\. Outbound Identity Preservation

Alumni may continue sending mail using their **@alumni.duke.edu** identity without Duke hosting their inbox:

* A lightweight outbound relay (Exchange Online or secure SMTP submission endpoint) accepts authenticated mail from the alum’s personal account.
* Duke’s DKIM/DMARC/SPF signing is applied.
* Mail is delivered with a Duke‑branded From: identity.

This is the same pattern used by Harvard’s **@post.harvard.edu** and MIT’s **@alum.mit.edu** identities.

\---

## 4\. Elimination of User‑Controlled Forwarding

Forwarding rules are disabled tenant‑wide. Because alumni no longer have mailboxes, they cannot create forwarding rules. Routing is handled centrally and predictably by Duke IT.

This directly resolves the “unpredictable rejection of forwarded messages” cited in Duke’s FAQ.

\---

## 5\. Modern Anti‑Spoofing Alignment

Because Duke controls both routing and outbound relay, the university can ensure:

* DKIM signing
* DMARC alignment
* SPF consistency
* ARC sealing (optional)
* Improved deliverability across major providers

This avoids the forwarding‑related failures that prompted discontinuation discussions.

\---

## 6\. Operational and Financial Efficiency

The routing‑only model requires:

* No additional Microsoft licensing
* No mailbox storage or litigation hold retention
* No MFA or password resets
* No new infrastructure
* Minimal administrative overhead

It leverages capabilities already present in Exchange Online.

\---

## 7\. Peer Institution Adoption

The following universities use routing‑only or mail‑enabled‑user alumni identity models:

* **Harvard University** — @post.harvard.edu
* **Stanford University** — @alumni.stanford.edu
* **MIT** — @alum.mit.edu
* **University of Pennsylvania** — @alumni.upenn.edu
* **Princeton University** — @alumni.princeton.edu
* **Dartmouth College** — @alumni.dartmouth.edu
* **Brown University** — @alumni.brown.edu

These institutions faced similar deliverability and security concerns and resolved them without discontinuing alumni identity.

\---

## Conclusion

The Exchange Online routing‑only model provides a secure, low‑overhead, and widely adopted alternative to discontinuing alumni email identity. It preserves the institutional value of **@alumni.duke.edu**, eliminates forwarding‑related failures, and aligns with modern security and deliverability standards.

Duke can implement this model using existing infrastructure and licensing, ensuring alumni retain a meaningful and durable connection to the university.



\---

For additional clarification or discussion regarding this model, feel free to contact  
**marc@merware.com** or **seinfeld@alumni.duke.edu**.

