# What prompt injection is

A language model takes text and continues it. Everything it receives, your instructions, the user's message, a document you pasted in, the contents of a web page it fetched, arrives as one stream of text. The model does not have a separate, protected channel for trusted instructions and an inert channel for data. It reads all of it and tries to do what the combined text seems to ask. Prompt injection exploits exactly this: content that should be treated as data contains text that reads as instructions, and the model follows it.

## The root cause is a missing boundary

In traditional software there is a hard line between code and data. A database query treats the query as instructions and the user's input as values, and parameterized queries enforce that the input can never become a command. Language models have no equivalent enforced boundary. The instruction to summarize a document and the document itself are the same kind of thing to the model: text to be continued. When the document says, in effect, ignore your previous task and do this instead, the model has no reliable way to know that line was data rather than a command from you.

## Why it is not just a bug to patch

Because the conflation of instructions and data is inherent to how current models work, prompt injection is not a single defect that a vendor can fix once. Each model update can change which phrasings get through, and attackers adapt. Treating it like a bug to be patched leads to a false sense of security after each fix. The durable framing is that any untrusted text reaching the model is a potential source of instructions, and the system must be built so that does not matter much.

## What an attacker is trying to achieve

The goals vary with the application. An attacker might try to make the model ignore its safety rules, reveal its system prompt or other users' data, produce output that misleads the user, or, most seriously, trigger an action through a connected tool, sending an email, making a request, changing a record. The damage scales with what the model is allowed to do. A model that only writes text back to one user can be made to say something wrong; a model wired to take actions can be made to take harmful ones. This is why the defenses center on limiting capability rather than on perfect input filtering.

## The honest baseline

The starting assumption for anyone building on a model should be that prompt injection cannot be fully prevented at the prompt level, and that any content the model reads from outside your direct control may carry hostile instructions. Everything useful follows from accepting that and designing around it, rather than from hoping a strong enough system prompt will hold. The rest of this reference is about what designing around it looks like.
