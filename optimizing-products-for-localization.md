---
layout: single
title: Optimizing Products for Localization
---

*This guide was written for product and engineering teams at a clinical SaaS company during a major expansion of the global localization program. It was distributed internally to build shared understanding of where localization fits in the SDLC and what upstream decisions most affect downstream outcomes.*

---

SaaS localization doesn't usually fail because of translation quality. It fails because product and engineering treat it as an afterthought.

Translators sometimes screw up, and I'm not saying that they don't. But the pattern is consistent: in project after project, when you take out upstream issues with product functionality and source files, the fraction of errors traceable to bad translator judgment is very low.

Those upstream issues often remain hidden until after code freeze, when they're expensive to fix and often delay release. But the good news is that they fall into predictable patterns and are easy to prevent if you take the right measures at the right time.

## Localization Team SLA

The localization workflow is our responsibility, and we'll do the following so it doesn't cause you headaches. (Refer to the Product Localization SLA for all the numbers and details about preconditions, project tiers, and quality assessment criteria.)

**Communicate:**

- Acknowledge project assignments on the Jira ticket within 12 business hours.
- Let you know immediately when we run into an impediment that threatens delivery.
- Respond within a business day to any escalation from you to the Localization Program Manager.

**Research:**

- Find answers to our queries without escalating to you whenever possible.

**Maintain quality:**

- Deliver with LQA score 7% above industry benchmark.
- Deliver with < 1% linter/testing/validation errors.
- Share monthly performance metrics through intranet.

The main factor we can't control is upstream: an MVP in English (US) may be impossible to localize into an MVP for other regions.

## Case Studies

In the moment, localization issues can seem like just the usual little fires to put out, but their cost adds up. These are a few cases from the last year in which incorporating global-readiness standards early in the SDLC would have saved real time and money.

### No Escape

**Issue:** If we don't escape syntax characters when using them in string values, the linter traps them as errors across all target languages.

**Case:** We received these English (US) source strings:

```
'xray.upload.success' : 'Your image was uploaded successfully.'
"xray.upload.failure" : "Your image didn't upload successfully. Please try again."
```

The translation-management system (TMS) returned these strings localized into English (UK):

```
'xray.upload.success' : 'Your image was uploaded successfully.'
'xray.upload.failure' : 'Your image didn't upload successfully. Please try again.'
```

There were 52 uses of (a single quote used as) an apostrophe in the English (US) source for the project. None had been escaped.

From there, a single offhand punctuation mark became an international incident. In translation into 61 of the target languages, it was either omitted or replaced with a curved/smart apostrophe, which wasn't read as a syntax character. The string key and value were thus automatically enclosed with single quotes by the translation-management system, and the file passed validation on the localization side.

But after delivery, the testing team got a report with 300+ errors, because their linter caught the different syntax characters and flagged them. While the issue was easy to spot, the correction and verification took five labor hours just before release.

### You Will Submit

**Issue:** Different regions may have idiosyncratic requirements that preclude the reuse of strings. You can't map product workflows to clinical-trial processes 1:1.

**Case:** The Clinical-Trial Management System (CTMS) product allows customers to manage required documents by uploading and "submitting", using the same key + value pair for the action on all screens.

That becomes a UX issue in Japanese, where there are at least two types of submission in clinical trials:

**Submission to notify (届出):** For example, transmission of the Clinical Trial Notification, after which the trial sponsor can proceed unless regulators follow up.

**Submission for approval (申請):** For example, transmission of the New Drug Approval request, after which the trial sponsor must wait for explicit approval before proceeding.

There is a more generic word used on send/submit buttons (送信), but it looks imprecise and unnatural in a purpose-built CTMS solution. Our pilot Japan customers complained.

The fix to the product involved creating new key + value pairs for the buttons on different screens, which the Japan localization team could then translate with more precision. Actually updating the resource file in the repo didn't take long, but the Jira comments, messages, and calls involved in getting the fix on the backlog took six colleagues a collective twenty hours of labor.

The issue would have been avoided if Japan had been included when requirements were generated.

### The Date from Hell

**Issue:** Non-localized date formats create the risk of entry errors that compromise data accuracy.

**Case:** In practice, we see this issue when localizing from English (US) into other languages, but its seriousness is more visible if we look at it in reverse.

Suppose we had a source product in Japanese that we were localizing into English (US), with its date-display format fixed in Japanese order. This is what would happen to the display:

**Japanese display:** 2026年4月14日（火）午後1:00

**English (US) display:** 2026Y4M14D (Tue) PM 1:00

That's about as good as you can get it, and it's still impossible to understand without rereading for several seconds. It's the kind of poor UX we deliver to other regions when we fix the date format into US everyday (April 4, 2026) or clinical-trial (04-APR-26) order. And because we're not yet past the year 2031, the year and day can be impossible to distinguish without examining the screen carefully for context. That's not the intuitive, frictionless UX we promise.

And it's error-prone. In the last year, we've received at least three requests to update live client databases (usually populated by log forms) because of errors discovered after form lock. Each request involves Customer Support, Professional Services, and Data Management, at an average of four hours of labor time. Lookback calls and studies of the feasibility of updating date formats pile on more time and effort.

Put tokens into placeholders and use ICU messaging to ensure dates display understandably in all languages, and the problem goes away.

### A Real Head Case

**Issue:** Concatenating and reusing strings seems like a short cut when you're coding, but it breaks the UI in other languages.

**Case:** As innocuous a piece of SaaS text as "If you encounter an error, please contact Customer Support" delayed an app release for three weeks because of its construction.

This is how it was provided for localization:

```
'form.intro.contactsupport' : 'If you encounter an error, please contact'
'contactsupport.link' : "%{supportLink}
```

Translators immediately noticed that the concatenation of strings forced them into English word order, but this was a rush project with multiple source-file updates, so the teams went with their best compromise.

The problem was that the actual text and link for the placeholder were in a different resource file that we'd localized three years earlier and not seen since. It was part of a list of standalone links used in page footers. In Ukrainian, that produced a grammatical error:

**Link text:** Служба підтримки

**Full sentence (as shipped):** Якщо виникла помилка, зверніться до Служба підтримки.

**Full sentence (correct):** Якщо виникла помилка, зверніться до служби підтримки.

Nouns don't have case inflections in English, but they do in Ukrainian (and a lot of other languages). The translation forced by the string structure is like saying, "Customer Support will help they." The error makes us look as if we couldn't get the most basic writing correct.

Additionally, sticking "Customer Support" at the end of the sentence isn't natural in languages such as Japanese and Korean. There are workarounds in those instances, but they look unnatural.

The quickest fix here was to put the link and its text within the string value for the message, which allowed translators to change word endings and order as needed to produce natural translations. Getting there involved reworking the source file and its translations in twenty languages, republishing to a lower environment for in-context review, and another full QA cycle.

## Where Localization Fails in the SDLC

When localization issues blow up at the eleventh hour before release, we all have to swarm, and the immediate fix is usually an unsatisfying stopgap that has to be fixed yet again in a subsequent release. But we can avoid all that if we anticipate global-readiness pain points from the start.

### Planning: Incorporate Global Workflows

When requirements are defined without input from regional stakeholders, the product doesn't adapt to regulatory and workflow differences in the target regions. At best, the translators have to force-fit their regional requirements into the UI as best they can. At worst, product and engineering have to rebuild features before they can be released.

In one reveal meeting, we were enthusiastically shown a new avatar design that was populated with the user's initials. The revelation that names in many languages don't have initials, rendering the avatar as a circle full of gibberish, was greeted less enthusiastically.

Involving regional stakeholders when defining user stories and use cases prevents such problems. It gets requirements from all supported regions incorporated into the product from the get-go.

### Design: Accommodate All Supported Languages

Designing screens around English (US) constraints means tying them to assumptions that aren't true for all supported regions. String length and formatting, icons, and field labeling vary among languages in ways that don't map cleanly. If the design doesn't adapt, we can end up with usability issues that lead to incorrect data entry (as when text is truncated, or an icon doesn't convey the intended meaning). Mobile apps are especially vulnerable to these kinds of problems.

The success screen for one of our patient-intake forms just had "Thank you!" as the header. The Korea team found even the most polite direct translations too curt (like "yeah, thanks, 'bye"). Good UX aligned with our brand required something like "제출이 완료되었습니다. 감사합니다. (Your submission is complete. Thank you.)" There was room on the mobile-app screen, but it took two releases to get the sizing changed. Until then, our UX had a dismissive tone for Korean users from their very first login session.

Adaptive design forestalls these issues by accounting for possible sizing, symbol, and labeling issues before localization begins, so translators can select the best text for each string without having to compromise.

### Engineering: Code for Global Readiness

Source strings that aren't localization-ready cause the biggest issues, for two reasons. One is that their issues are replicated across all target languages, so the cost in consultation and remedial actions mushrooms. Another is that they rarely have workarounds: translators can't localize them at all or have to torture them into awkward UX.

Translators and language QA specialists like using their expertise to solve sophisticated problems. They're not any more thrilled than your team is at query exchanges about blow-ups caused by apostrophes or noun endings. But when those mechanical issues become blockers, they have to be resolved.

We notify you of issues as soon as we notice them, but we can only see what's provided in the project assignment. We once got to QA before we saw that the (large, bold) "Your Dashboard" header displayed on one new screen in English no matter what language was selected. The string was hard-coded in English (US), so it wasn't in the resource files we had access to. And the screenshots we'd received were outdated, so we didn't know it had been added to the design. Fixing that one took a week.

Enforcing global-readiness standards in code and resource files is relatively straightforward. Run localizability checks before translation and before issues have a chance to propagate.

### Pre-release: Provide Visual Context

UI strings make no sense without context. "Form Submitted" could be an audited action, a header for a date column, a status indicator, a success message in a toaster, or a label for a field populated with the form name. Keys sometimes help a little, but without visual context, translators have to guess at meaning.

Guessing is risky, so the only alternative is to open a query, which is often the first volley in a series of clarifications over several business days. Our query-management system makes questions and answers available to all linguists on a project, but there's no guarantee that everyone sees them and applies them consistently. The result is extra work, delays, and inconsistency.

Providing environment access or wireframes at project kickoff eliminates the need for all but a tiny proportion of queries. And by ensuring that translators get the UI right for the first delivery, it prevents troublesome string revisions in subsequent releases.

### Release: Give Customers a Plan

We're an agile company, and imperfections will surface in releases sometimes even if we follow all the rules for global readiness. Small post-release fixes only become large headaches when we don't plan for them on the spot. Customers hate the feeling that they've discovered an issue we weren't already aware of; it makes them wonder how good our QA and testing are. They also hate being told a bug is in limbo, on the backlog without a firm release date for the planned fix. What happens next is a long series of follow-ups that drag in Customer Support and Professional Services, with customers left feeling that we don't address their concerns unless they nag us.

The key to avoiding this drama is to show that we're aware of problems and are doing something about them before we have to be asked. If a release must go ahead with a localization issue included, create a ticket in Jira immediately, and be sure the Tech Comm lead has added it to the Known Issues list. Enter a targeted fix date on the ticket, even if the product manager says it has to be a few release cycles in the future. Knowing a problem is scheduled to be fixed takes a lot of the sting out of it for customers (and for customer-facing colleagues).


# Contact
[LinkedIn](https://www.linkedin.com/in/sean-kinsell/)

[Email](mailto:skinsell@gmail.com)
