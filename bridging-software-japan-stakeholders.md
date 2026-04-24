---
layout: single
title: "Bridging Software Development Teams and Japanese Stakeholders"
---

*Adapted from a talk I gave at Vistatec's invitation at LocWorld 38. I've left these in speaker-note form, as I actually used them.*

*If Japanese is one of your target locales, you probably recognize most of the frictions below. My audience was localization program managers, so I spoke from their point of view and focused on what they can do. But I’ve been a Japan-based customer myself; I’ve also experienced the same frictions from the other direction.*

*So this piece shares what I've learned working across these dynamics from both sides, and offers practical steps you can take right now as a loc manager. Many of these points are helpful for any language pair, though I’m discussing them specifically in the context of Japan.*

---

## The Friction Points

If one of your target languages is ja-JP, you’ve probably had to deal with the following:

- “We’re an agile company, but Professional Services complains if release one isn’t perfect.”
- “Having to explain to clients why the product doesn’t work puts us in a tough spot.”
- “We decided this months ago—why do they say we need to revisit it?”
- “We need source-of-truth documentation, not just emails and meeting notes.”
- “We don’t understand why you keep asking for more information when we already have milestones in jeopardy.”

Note that I’ve mixed together complaints from and about the Japan side to highlight that they tend to revolve around the same themes. Most of these issues come from ambiguity in decisions, communication, and ownership. They can come up even on relatively straightforward projects.

A major factor is misaligned incentives. It’s not that Product teams don’t care about CSAT, or customer-facing Japanese colleagues don’t care about velocity. But they often disagree, especially on agile projects, about where compromises should be made.

- **Product:** Get to MVP and publish  
- **Japan customer-facing:** Fulfill all customer requirements before publication  

---

## Find Common Ground

Here’s a reframing that can help: If you’re a localization-program manager, your Japan offices have the same problems you do:

- They’re treated as downstream from development.
- They feel removed from informal real-time networks.
- Bad localized work product increases pain and LOE (in QA for you, in CSAT for them).

That shared position is your starting point. The goal isn’t to manage a difficult stakeholder — it’s to find a colleague with aligned interests and build toward that.

---

## What Localization Managers Can Do

- Communicate functionality/delivery changes early. Don’t wait. Professional Services/Customer Success in Japan needs to assure customers that the transition will be smooth and that risks are accounted for.

  - **Ideal:** ≥ 6 weeks’ lead time. Customer-facing colleagues can adapt information and messaging, communicate it, and address concerns before implementation date.  
  - **Realistic:** As soon as you hear of them. Japanese colleagues will be grateful if you ask the change owner for the reason for the aggressive timeline and how risks are being managed.

- Connect Japanese colleagues to the language lead. Stakeholders feel more confident in people they know, and they like being able to tell customers, “I’ve spoken to the translators about the point you raised…”.

  - **Intro:** In-person is best, but a video or phone call works if necessary. The language lead can decide whether to include other linguists on the team. Provide the resume/CV for the language lead ahead of time. If the language lead has developed style guides or other references, send them also. You want to set your language lead up as a trustworthy partner.

  - **Alignment calls:** If your Japan office is receptive, set up a monthly touch point between a Japanese stakeholder and the language lead. Even if they can only spare 15 minutes, regular contact helps build trust and provides an opening to talk about new terminology, the results of an LLM assessment, and other topics that may not come up in more formal communications until later.

- Stay connected yourself. The more you know, the more you can position yourself as a trusted partner and help avoid issues. Share new information as well as asking for it.

- Check in regularly and informally with Professional Services, Customer Success, Sales Enablement, Technical Communication, Training, and Marketing to pool and align your product-related content.

- Make sure you’re executing all the workflows InfoSec, Compliance, and Process/Validation require for your tools, vendors, processes.

- Talk to Product, Engineering, UX/Design, and Data Science about features that are likely to be added to the roadmap.

- Document everything. Having transparent processes, content standards, and project tracking gives stakeholders confidence that you know what you’re doing.

- Work with whoever governs processes to create SOPs for localization workflows. (It’s good to demonstrate willingness to make your processes auditable.)

- Work with whoever owns the PDLC to integrate global readiness and loc criteria. (You may have to keep at this for a while, but it helps with getting loc issues into the risk register, requirements-traceability matrix, roles/responsibilities, and work breakdown structure, however they’re recorded in your workflows.)

---

## What You Can Ask of Product/Engineering

It’s reasonable for your Japan colleagues to expect to be looped in and to have input on things that will affect their customers.

As a loc program manager, you’re often asked to create new customer-facing work product with little lead time and context. Ask your Japan colleagues to partner with you on pushing to have global readiness embedded in the PDLC from beginning to end, which solves a lot of problems before they start. They should also partner with you on the current project: escalating problems that originated upstream, asking for missing context and reference materials, and clarifying nebulous requirements.

Some of the items below are just changes of mental framework; others may become part of internal SLAs. And several of them are helpful to product quality, processes, and localization in general—not just in making Japanese stakeholders happier.

- Create globalization-ready products.
- Show the value of global readiness to Product and Engineering.
- Address:
  - Concatenations/placeholders/variables  
  - Embeds/externalization  
  - Fixed formats/ICU messaging  
  - Syntax characters as text in string values/escaping  
  - String length and screen constraints  
  - Key+value pair reuse  
  - Term consistency  

- Have a plan for defects:
  - Known-issues lists  
  - Confirmed fix timelines  
  - Framework for defects in MVP  

- Meet Japanese stakeholders in the middle.

To surface miscommunications:

- Confirm meaning explicitly in writing if feedback is ambiguous.  
- Respond to ambiguous feedback as undecided, not as indirect agreement.  

Helpful reframing questions:

- “What improvements to the plan or additional considerations do your customers need?”  
- “Who will communicate the final decision to us?”  
- “We’ve talked about X, Y, and Z. What other criteria will you use to decide? What inputs do you need from us after the call?”  
- “We have to keep a tight timeline for this cycle. What support will help you not feel so rushed in future cycles?”  

---

## What You Can Ask of the Japan Stakeholders

- Specify a source of truth for decisions and an escalation path.
- Provide interim decisions to work from.
- Sell successes to customers.
- Adhere to timelines.
- Document style preferences.

---

## Where to Start

If you’re looking for a quick-start path, I’d do these things first:

- Talk to the PDLC process owner about incorporating localization.  
- Bring the Japan team and the Japanese linguists together.  
- Focus on defects that escape to production.  

---

Questions and comments welcome.


# Contact
[LinkedIn](https://www.linkedin.com/in/sean-kinsell/)

[Email](mailto:skinsell@gmail.com)
