# Prompt Injection Defense Reference

Prompt injection is when text that a language model reads as input contains instructions that hijack what the model does, overriding the developer's intent. Because a model cannot reliably tell the difference between trusted instructions and untrusted content that looks like instructions, you cannot fully solve it with clever wording. You contain it with architecture: limit what the model can do, treat all model output as untrusted, and keep a human or a hard rule between the model and anything that matters.

This is a defensive reference for people building applications on top of language models.

## What is inside

- [01-what-prompt-injection-is.md](01-what-prompt-injection-is.md) why models conflate instructions and data
- [02-direct-vs-indirect-injection.md](02-direct-vs-indirect-injection.md) the two delivery paths and why indirect is worse
- [03-defenses-that-help.md](03-defenses-that-help.md) the controls that actually reduce risk
- [04-defenses-that-dont.md](04-defenses-that-dont.md) the ones that feel safe but are not
- [05-designing-for-least-privilege.md](05-designing-for-least-privilege.md) building so a successful injection does little harm

## More

Part of a catalog of single-file browser tools and plain-language references, all MIT licensed and dependency-free: [0xelitesystem.github.io](https://0xelitesystem.github.io/). Built by [elitesystem.ai](https://elitesystem.ai).

## License

MIT. Copyright (c) 2026 RBL Industries LLC.
