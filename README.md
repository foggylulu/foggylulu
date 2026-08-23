### Luca Fogel Robion

I work at the intersection of **AI, mathematics and building things that ship** —
mostly instruments and tools for language models running on hardware I control.
Northeastern '28, BS Business Administration and Mathematics. Looking for an
AI/ML co-op from September 2026.

The through-line in everything below is one rule: **a measurement is a number;
the absence of one is `—`.** Most of the code in these repos exists because some
model or some API reported a plausible wrong value and something had to refuse it.

| | |
|---|---|
| [**mac-ai-activity**](https://github.com/foggylulu/mac-ai-activity) | Live instrument panel for local AI inference. Joules per token, per-slot llama.cpp telemetry, SMC die temps and fans. Observe-only, native SwiftUI, no dependencies. |
| [**llmctl**](https://github.com/foggylulu/llmctl) | Which local models are holding your Mac's RAM, and one command to get it back — including the idle server processes still pinning weights with `mlock`. |
| [**wake-on-llm**](https://github.com/foggylulu/wake-on-llm) | Turn a big local model on and off from your phone, from anywhere, over Tailscale. The last-resort path is a literal Wake-on-LAN packet. |
| [**solvr-ai**](https://github.com/foggylulu/solvr-ai) | Native iOS and Mac Catalyst maths tutor: photograph a problem, get the working and the lesson behind it. One interface over a hosted API, a local server, or Apple's on-device model. Built and working, not released. |

Solvr came out of my own tutoring practice — I have taught calculus and statistics
since 2022 and kept rebuilding the same lesson plans by hand.

A few things these projects measured that are not written down elsewhere:

- **`"\frac{d}{dx}"` is valid JSON.** `\f` is a form feed, so a strict parse
  succeeds and silently returns mangled maths with no error raised. Any pipeline
  that repairs model output only *after* a failed parse is corrupting every
  fraction it is given.
- **`n_decoded` is not reset at request start.** Through the whole of prefill it
  holds the previous request's count — long enough to produce a confident,
  entirely fictional token rate.
- **A parked CPU cluster reads colder than the room.** 113 °C during a CPU burn,
  23 °C sixty seconds later while the GPU sat at 82 °C. A headline labelled "CPU
  temperature" therefore *falls* as the machine gets hotter.
- **A 46 GB model in the page cache is not memory pressure.** Count it as used
  and a machine with 40 GB free looks full.
- **Asking a router a question can cost 3 GB.** One `GET /props?model=…` against
  an idle llama.cpp router loads the model. An observer that can do that is not
  an observer.
- **A fine-tune can learn the shape at the expense of the content.** A LoRA on a
  4B improved schema compliance and cost nineteen points of maths correctness,
  because it learned to emit one compliant step instead of working the problem.
  Schema compliance is blind to that — a one-step answer that decodes cleanly
  scores 100%.
- **Removing one flag was worth 30x.** `--enable-memory-reclaimer` against a
  model sitting at 99.7% of the GPU budget meant "evict" was always true, so
  every request paid a reload: warm time-to-first-byte 22 s → 0.7 s once it was
  gone.
