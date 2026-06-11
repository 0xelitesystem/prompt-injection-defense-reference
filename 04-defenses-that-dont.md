# Defenses that do not work

Several defenses feel reassuring and are widely attempted, but they do not reliably stop prompt injection. Relying on them produces confidence without safety, which is worse than knowing you are exposed. It is worth understanding why each fails so you do not lean on it.

## Telling the model to ignore injection

The most common attempt is to add a line to the system prompt instructing the model to ignore any instructions found in user input or documents. This helps a little against the laziest attempts and fails against real ones. The reason is fundamental: the instruction to ignore injected instructions is itself just text, and the injected instructions are also text, and the model weighs them as a continuation problem rather than as a security boundary. A determined injection can be phrased to take precedence, to claim higher authority, or to convince the model the situation has changed. You cannot reliably out-instruct an attacker who controls part of the input, because you are both writing to the same channel.

## Filtering for known bad phrases

Blocking inputs that contain phrases like ignore previous instructions catches yesterday's attacks and misses tomorrow's. Natural language has unlimited ways to express the same intent, across languages, encodings, paraphrases, and indirection. A blocklist is always behind, and the false sense that it is catching things can be worse than having nothing, because it encourages connecting more capability behind a filter that leaks. Filtering can be one thin layer, but it is not a boundary.

## Delimiters and special markers alone

Wrapping untrusted content in delimiters or special tokens, and telling the model that anything inside is data, is better than nothing but not a real boundary. Attackers can include the closing delimiter in their content to break out, or simply write instructions that the model follows despite the wrapper. Delimiters help the model organize the prompt; they do not enforce that it will refuse to act on what is inside. Treat them as formatting, not as security.

## Trusting a model to classify its own inputs

Asking a model whether a given input contains an injection attempt, and proceeding if it says no, inherits the same weakness: the classifier model can itself be injected by the content it is examining. Using a model to detect attacks on a model adds a layer that the attacker can also target. It can be a useful signal as part of monitoring, but it is not a gate you can fully trust, because the thing checking for hostile instructions reads those instructions too.

## Why these matter less than capability limits

The thread running through every failed defense is that they try to stop the model from being fooled, when the model can always be fooled by something. The defenses that work do not assume the model stays uncorrupted; they assume it will sometimes be corrupted and make that not catastrophic. If your safety depends on the model never following an injected instruction, you are relying on something that cannot be guaranteed. If your safety depends on the model not being able to do much harm even when it does follow one, you are relying on architecture you control. Put your effort in the second place.
