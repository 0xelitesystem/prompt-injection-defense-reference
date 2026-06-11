# Designing for least privilege

Because injection cannot be fully prevented, the goal of a well-built system is that a successful injection accomplishes little. That is what least privilege means in this context: every part of the system has the minimum capability and access it needs, so a hijacked model finds few levers to pull. This is a design discipline, applied before the model is connected to anything.

## Start from what could go wrong

Before connecting a model to a tool or a data source, work backward from the worst outcome. If the model can send email, the worst case is it sends hostile or data-leaking email to anyone. If it can query a database, the worst case is it reads or changes records it should not. If it can run code, the worst case is whatever that code can do. Listing these honestly, assuming the model's output is fully attacker-controlled, tells you where the real exposure is. Capabilities that survive this examination need guards; ones that do not should be removed or fenced.

## Scope access tightly

Give each model call access only to the data and tools that specific call needs, not to everything the application can reach. A call that summarizes a public document needs no access to user records, no tools, and no secrets, so a successful injection in that document reaches nothing. A call that drafts a reply needs the conversation but not the ability to send. Narrow scopes mean that even a fully hijacked call is boxed into a small area. Broad scopes mean one injection reaches everything, which is the situation to avoid.

## Separate reading untrusted content from taking action

A recurring safe pattern is to keep the step that reads untrusted content separate from the step that acts. One call, with no tools and no sensitive access, reads and processes the external content and returns a structured, validated result. A separate, controlled step decides whether to act on that result, applying its own rules rather than trusting the first call's reasoning. The untrusted content never sits in the same context as the ability to do something consequential. Breaking the chain between read and act is one of the most effective structural defenses.

## Validate everything the model emits

Treat the model's output as a proposal to be checked, not a command to be executed. A proposed recipient is checked against an allowlist. A proposed amount is checked against a cap. A proposed query is constrained to safe operations. A proposed file path is confined to an allowed directory. The validation lives in your code, where an injection cannot reach, and it enforces the limits regardless of how the model was persuaded. This is the same principle as never trusting user input in traditional software, extended to the model itself.

## Keep a human in the loop where it counts

For the actions whose worst case is genuinely costly, the most reliable guard is still a person confirming before it happens, shown clearly what is about to occur. This is not always practical at scale, which is why the automated guards above matter, but for the highest-stakes actions it is the strongest defense, because a human can notice that the proposed action does not match the user's intent in a way no automated check fully can. Design so that the rare consequential action pauses for confirmation, while routine harmless actions flow freely, and you get most of the safety for little of the friction.
