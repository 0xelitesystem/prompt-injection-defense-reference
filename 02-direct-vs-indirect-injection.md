# Direct vs indirect injection

Injection arrives by two paths, and the second is the one that catches teams off guard. Direct injection comes from the person typing to the model. Indirect injection comes from content the model pulls in from somewhere else while doing its job. Both override intent, but they have very different threat profiles.

## Direct injection

In direct injection, the user themselves writes the hostile instructions, trying to make the model break its rules or reveal what it should not. Because the user is the attacker and also the audience, the worst they can usually do is harm their own session: get the model to ignore a guideline, role-play around a restriction, or expose the system prompt. This matters for safety and for protecting your instructions, but the blast radius is often limited to that one user manipulating their own interaction.

Direct injection is also the easier case to reason about, because you know the input came from the user and you can treat the whole conversation as untrusted from that user's perspective. You are mainly protecting your own policies and other users, not protecting the user from themselves.

## Indirect injection

Indirect injection is more dangerous because the hostile instructions are hidden in content the model reads as part of a legitimate task, placed there by someone other than the current user. A model that summarizes a web page, reads an email, processes an uploaded document, or pulls from a knowledge base can encounter instructions embedded in that material by a third party. The current user did not write them and may not know they are there. The model reads the page, encounters the embedded instruction, and may act on it as if it came from the user or the developer.

The reason this is worse is that it breaks the assumption that the user controls the model's inputs. An attacker can plant instructions in any content your system might fetch, and then wait for a victim to run a normal task over it. The victim asked for a summary; the planted text tried to make the model exfiltrate their data or take an action on their behalf. The user is now a vector against themselves, and the attacker never touched your system directly.

## Why connected tools raise the stakes

Indirect injection becomes severe when the model can act, not just read. If the same model that reads an untrusted document can also call a tool to send a message, make a purchase, or modify data, then planted instructions in that document can reach those tools. The path runs from attacker-controlled content, through the model, to a real action, without the user intending any of it. Many of the most concerning real-world scenarios combine an agent that reads external content with an agent that can take consequential actions.

## The design implication

The two paths point to the same conclusion from different directions. For direct injection, treat the user's input as untrusted and protect your policies and other users. For indirect injection, treat every piece of external content the model reads as potentially hostile, and never let reading untrusted content flow straight into a consequential action. The defenses that follow apply to both, but indirect injection is the reason the architectural controls, not the prompt-level ones, are what actually keep you safe.
