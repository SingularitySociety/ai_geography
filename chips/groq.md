

https://www.semianalysis.com/p/groq-inference-tokenomics-speed-but

Groq’s chip has a fully deterministic VLIW architecture, with no buffers, and it reaches ~725mm2 die size on Global Foundries 14nm process node. It has no external memory, and it keeps weights, KVCache, and activations, etc all on-chip during processing. Because each chip only has 230MB of SRAM, no useful models can actually fit on a single chip. Instead, they must utilize many chips to fit the model and network them together.

