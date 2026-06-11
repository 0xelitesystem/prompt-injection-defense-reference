# Defenses that help

There is no single fix for prompt injection, but a layered set of controls reduces both the chance of a successful injection and the damage one can do. The most effective defenses are architectural: they assume injection will sometimes succeed and limit what that success can accomplish.

## Least privilege for the model

The most important control is to give the model the least capability it needs. A model that only returns text to one user can do little harm even when injected. Every tool, permission, and piece of data you connect expands what a successful injection can reach. Before wiring a model to an action or a data source, ask what an attacker who fully controlled the model's output could do with that connection, and assume they will. If the answer is unacceptable, the connection needs a guard or should not exist. This single principle does more than any prompt wording.

## A human or a hard rule before consequential actions

For any action that matters, sending something, spending something, deleting something, changing a record, put a checkpoint between the model and the action that the model cannot talk its way past. A human confirmation for high-stakes actions is the strongest version. A hard-coded rule that the model cannot exceed, such as a spending cap or an allowlist of recipients, is the automated version. The point is that the decision to take a consequential action does not rest solely on the model's say-so, because that say-so can be hijacked.

## Treat model output as untrusted

Output from the model should be handled with the same suspicion as input from a user. If the model produces text that gets inserted into a web page, it can carry a cross-site scripting payload, so it must be escaped. If it produces a command, query, or path, that must be validated against an allowlist, never executed directly. If it produces a tool call, the arguments must be checked. A successful injection often tries to make the model emit something dangerous downstream, so the layer that consumes the model's output is a critical place to enforce safety independently of the model.

## Separate trust levels and isolate untrusted content

Where you can, keep untrusted content in a clearly bounded place and limit what the model does with it. Structuring the prompt so that external content is presented as data to be analyzed, with the model instructed to treat it as untrusted, helps somewhat, though it is not sufficient alone. More effective is to use separate model calls for separate trust levels, so the call that reads a hostile document has no tools and no access to sensitive context, and only its sanitized result, not its raw instructions-following, flows onward.

## Monitoring, limits, and logging

Assume some injections will get through and instrument for it. Rate limits and spending caps bound the damage of a runaway agent. Logging the model's tool calls and actions lets you detect and investigate anomalies. Alerting on unusual patterns, a sudden burst of outbound actions, access to data outside the expected scope, catches what slipped past. These do not prevent injection, but they shrink how long and how far a successful one runs before you notice and stop it.
