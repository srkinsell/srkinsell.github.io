# One Bad Error Message

*How a translator query uncovered a UX problem nobody else had caught*

We once received this error message to localize: "Your operation was not completed successfully." That was it — the whole message. In its favor, it tells the user, about as elegantly as possible, that something didn't work. (Well, I suppose you could say that "successfully" isn't necessary, but that's arguable.)

I'm all for the principles of minimalist tech writing, but there's such a thing as too minimal. When this string landed in a localization project I was managing, I saw it in a query from the CJK translation teams. Their question was reasonable: *what operation?* In Chinese, Japanese, and Korean, how you refer to an operation can shift slightly depending on the action that triggered it. Without knowing whether the user had been trying to process a document, update their profile, or jump to the next page, the translators couldn't be confident they were choosing the most natural wording. So they asked.

I went to the product to look up the answer and, in the process, noticed something else. This string was the whole error message. It didn't tell users what to do. It just confirmed that something had gone wrong and left them hanging.

Something there didn't seem, shall we say, to be entirely in keeping with our goal of a friendly, frictionless UX. I sent a Jira comment to ask: what action does this message correspond to, and what should the user do to fix the problem?

That turned out to be more complicated than it seemed.

## Two Weeks, One String

What followed was fifteen Jira comments, six Slack messages, an impromptu meeting, and about two weeks of elapsed time before we had a resolution.

The answer to "what action?" was that the message was triggered if users with insufficient role permissions tried to upload a file past a certain phase in the approval workflow. Pretty straightforward, if hard to express economically. But the more important answer was the one that came back to "what should the user do to fix it?": *nothing*. The error reflected a backend condition the user didn't have permissions to alter. They needed to escalate to their administrator.

The updated string: *"Your document could not be added. Please contact your app administrator for help."*

Specific action. Clear next step. Translatable without ambiguity. (The Japan and Korea teams added an apology at the beginning, in line with customer expectations in their regions, but they were able to use the source text directly.)

## What the Query Actually Revealed

The localization review didn't cause this problem; it uncovered it. The original message had passed through writing, engineering, and QA without anyone flagging that it abandoned users at a dead end. The CJK teams' query was the first time anyone had asked, in effect: *what is this message actually trying to do?*

That's not unusual. In many ways, localization is QA. Translators have to pore over every word of source content, and they interact with it in runtime like a user. They don't have the product team's knowledge that, well, of course you can't upload a file here if you don't have the right permissions. When a string is too vague to translate confidently, it's often too vague in the original language too.

In this case, the query from the CJK teams was the forcing function that brought the UX problem to the surface. One intervention resolved both issues at once. We were able to capture the pattern represented by the issue in new protocols for notification writing, so we didn't have the same issue with future notifications.

## The Upstream Argument

The fix itself was straightforward. The two weeks it took to get there weren't wasted, exactly, but they were avoidable.

We see again and again the cost of treating localization as a downstream function rather than as a partner throughout the SDLC. Localization catches problems late that could have been prevented early. The cost isn't just translation rework; it's the accumulated time of everyone pulled into the thread. Multiple exchanges of messages and a meeting on the fly represent a lot of people pausing to fuss over something that was supposed to be finished. But the translators didn't cause the trouble. They just identified and escalated it so it could be fixed. If localization-readiness criteria had required error messages to specify a trigger and a corrective action, the issue would have been prevented before the source string was even written.
