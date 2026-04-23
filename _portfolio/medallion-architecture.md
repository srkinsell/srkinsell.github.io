---
layout: single
title: Medallion Architecture in Our Data Flow
---

*This explainer was written for customer-facing teams at a wealth management SaaS company to demystify a technical concept that frequently came up in onboarding conversations with clients. The goal was to give non-engineers a working mental model they could use in the field — accurate enough to be credible, accessible enough to actually get read.*

---

We talk a lot about "medallion architecture" in customer-onboarding meetings nowadays. That name isn't an obvious metaphor for data processing, so it may sound confusing. But the underlying principles are straightforward.

## Problem

The most obvious way to work with data is to receive it from customer sources, load it into our data store, and start feeding reports and dashboards with it. If we try that in practice, we run into issues almost immediately:

- The same information can be labeled differently in different sources (for example, "first name" vs. "given name").
- Data can change in real time, but we may only receive it at intervals (for example, bank balance changes because of deposits and withdrawals).
- There may be items missing from the data the source gives us.

In those cases, anything we compile from the data will have inaccuracies, conflicts, and gaps.

## Medallion Architecture

Medallion architecture solves the problem by addressing it in phases. At the end of the process, the data is complete, organized, and ready to use.

The medallions represent progressively better performance:

**Bronze:** Raw data (as it was received). Checked to make sure nothing is missing or unreadable.

**Silver:** Cleaned and organized data. Processed to remove errors, make all the information consistent, and align it with our schema (how our data store defines field labels, permissible formats for values, rules values must follow, and other properties).

**Gold:** Data ready for business use. Shaped and grouped to be ready for our reporting and viewing systems.

The advantage of processing data in these phases is that you can trace problems easily as they arise, because you're making changes in a set order. If you try to get data ready to import into dashboards while you're also cleaning it, you're applying lots of types of changes at once, which makes it hard to find specific causes when errors occur.

### 1. Bronze

At the bronze level, you treat the data shipment the way you would take in a delivery of physical packages: verify that it arrived complete and intact, and retain it exactly as received. If there are damaged or missing items, you get more shipments until the order is complete. Once it is, you've earned the bronze medal.

This may sound like a formality, but data loads go wrong in all kinds of ways. If you use an API call, there may be caps on numbers of rows. The schema at the data source may have changed since the last shipment without any warning. The customer may have "required" fields that aren't actually populated. Getting all the rows and values you expected can take a surprising number of tries.

Keeping a copy of the data as you received it ensures that, if it gets too messy to recover while you're working with it later, you can always start over with a fresh copy. You don't have to waste time trying to reconstruct it or waiting for a reshipment.

### 2. Silver

Now that you have all the data in readable form, it's time to clean it up. The goal is to apply your schema to the data. That's complex, and the risk of failure is high, so this phase takes the most time and effort.

Financial data introduces some uncommonly stubborn variables — for example, determining the timestamp of a trade. It's usually clear when it was put through, but different custodians calculate the settlement date differently. If a trade was supposed to be settled while they were closed, they may backdate it "as of" the planned date when they process it later. Or they may not, which means that the settlement is recorded later than expected. Calculating the net worth at a point in time for a client with a global portfolio can start to feel like metaphysical inquiry. The only way to accuracy is to track settlements by current status and normalize them to our organization-wide calendar.

When the data is organized and stable, it earns the silver medal. At this point, the data is aligned with your schema, and your data store should be able to ingest it without errors.

### 3. Gold

Data aligned to your schema is ready to have business logic applied. Business logic takes isolated pieces of data and puts them together so they mean something to users.

You need clean data from the silver level to do any gold-level work, which usually involves our internal definitions of metrics. The same position and pricing data (account eligibility, valuation timing, and aggregation logic) could yield different AUM figures for an advisor depending on whether we include cash, or what we accept as the effective dates of transactions.

At the gold level, you combine data points into metrics and structure them so they're ready to be used in product features and dashboards. This is where you test whether everything is working predictably: does the same business-logic calculation give you the same value every time across systems? Once you've verified that, the data gets the gold. It's ready to use in applications.

## Medallion Architecture in Practice

Medallion architecture is only a framework. Real projects that apply it can still break down. In broad terms, these are the usual causes:

**Loose validation gates.** At every level, the team needs to verify that the data received fulfills the criteria for the previous level. Checks on referential integrity find ghost relationships all the time: trades attributed to accounts that weren't active, or transactions with outdated currency codes. If those aren't correct, you can't go from silver to gold.

**Multiple updates at once.** It's tempting to start organizing data into reports while you're still cleaning it up, but the result is inaccurate results that may include duplicates or invalid values. Determining the root cause of those breakdowns can be painful, especially because ownership of individual transforms may not be clear.

Also, medallion architecture tells you *when* certain actions should be taken; it doesn't tell you what decisions they should be based on. Figuring out systems of record, persistence criteria, and what inputs are used for derivations requires judgment. Bad judgment will produce bad results and delays, but medallion architecture will help to figure out where things went wrong and what changes are likely to help.
