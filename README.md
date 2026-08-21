### Luca Fogel Robion

I build instruments for local AI on Apple Silicon — tools that answer questions
the engines themselves don't: what a generation cost, what is holding the RAM,
and whether the number on screen is a measurement or a guess.

The through-line in all three is the same rule: **a measurement is a number; the
absence of one is `—`.** Most of the code in these repos exists because some
engine reported a plausible wrong value and something had to refuse it.

| | |
|---|---|
| [**mac-ai-activity**](https://github.com/foggylulu/mac-ai-activity) | Live instrument panel for local inference. Joules per token, per-slot llama.cpp telemetry, SMC die temps and fans. Observe-only, native SwiftUI, no dependencies. |
| [**llmctl**](https://github.com/foggylulu/llmctl) | Which local LLMs are holding your Mac's RAM, and one command to get it back — including the idle server processes still pinning weights with `mlock`. |
| [**qwen-control-mac**](https://github.com/foggylulu/qwen-control-mac) | Turn a big local model on and off from your phone, from anywhere, over Tailscale. |

A few things they measured that aren't written down elsewhere:

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
- **Removing one flag was worth 30x.** `--enable-memory-reclaimer` against a
  model sitting at 99.7% of the GPU budget meant "evict" was always true, so
  every request paid a reload: warm TTFB 22 s → 0.7 s once it was gone.

Interested in AI systems, quantitative finance, and the places where the two
meet. Currently studying, and looking for co-op work from September 2026.
