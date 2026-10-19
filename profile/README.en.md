# Phono Project

English | [简体中文](README.md)

**Open infrastructure for Chinese phonetic input methods.**

Phono Project aims to decompose Chinese input technology from a closed product capability into open, composable, researchable, and deployable building blocks—from syllable parsing for continuous pinyin, through context-aware Chinese character generation, to local inference on mobile and embedded devices.

The current implementation focuses on tone-free full pinyin and uses pinyin syllables as the intermediate representation between layers. This boundary is intended to let full pinyin, double pinyin, and other schemes that can be mapped to pinyin syllables share segmentation, context modeling, candidate generation, and on-device deployment capabilities in the future.

> Phono Project is not a complete input method product. It is an open foundation for building Chinese phonetic input methods.

## Why Phono Exists

Chinese input methods connect keyboard encodings, linguistic knowledge, user context, and device systems, yet these core capabilities are usually enclosed within complete products. Meanwhile, open research on models, data processing, and inference is often fragmented, leaving a substantial gap between individual results and an input pipeline that can actually be integrated, deployed, and continuously improved.

Phono Project seeks to narrow that gap. We treat research models, deterministic algorithms, runtime interfaces, and on-device engineering as different layers of the same problem, and work to establish clear, stable, and independently replaceable boundaries between them.

## Design Principles

**Open.** Models, algorithms, interfaces, and implementations should be available for study, verification, and extension instead of existing only as invisible product capabilities.

**Composable.** Syllable parsing, P2C modeling, candidate decoding, and runtime execution should retain clear boundaries so that research and applications can select, replace, or reuse individual components.

**On-device first.** Local inference is not an afterthought added after a cloud prototype; it is a primary scenario that shapes model architecture, cache strategy, quantization, and interface design from the beginning.

**Research into practice.** A method should not only be trainable and measurable; it should also be exportable, integrable, testable, and usable in a real input interaction pipeline.

## Projects

### [PhonoP2C](https://github.com/phono-project/PhonoP2C)

PhonoP2C is a research project for context-aware pinyin-to-Chinese conversion. It uses a two-stage encoder-decoder architecture to generate Chinese character candidates from Chinese history and pinyin syllables, covering corpus preprocessing, model training, evaluation, ExecuTorch export, and model package assembly.

### [Phono-PinyinSegment](https://github.com/phono-project/Phono-PinyinSegment)

Phono-PinyinSegment is a lightweight pinyin gap-scoring model. It scores boundaries between adjacent characters in continuous pinyin, while globally constrained decoding is performed over a Trie DAG induced by the legal pinyin vocabulary instead of making independent greedy decisions at each gap.

### [phono-core](https://github.com/phono-project/phono-core)

phono-core is the C++ inference engine for on-device deployment. Built on ExecuTorch, it combines pinyin normalization, intelligent segmentation, legal-path decoding, context and KV-cache management, candidate generation, and beam search, exposing these capabilities to mobile, embedded, and language-binding integrations through a stable C ABI.

## Current Pipeline

Together, the three projects cover the main path from model research to on-device execution: continuous pinyin is first normalized and modeled for syllable boundaries, then decoded into a syllable sequence under the constraints of a legal pinyin vocabulary; the P2C model combines that sequence with committed Chinese context to generate character candidates; finally, the C++ runtime handles caching, decoding, context reuse, and application integration.

## Current Scope

The current implementation focuses on tone-free full pinyin, Simplified Chinese candidate generation, and on-device inference. Double-pinyin key mapping, a complete input method frontend, user dictionaries, long-term personalization, and cross-device synchronization belong to the broader project vision, but should not be understood as ready-to-use features available today.

The project remains under active development and research. Interfaces, model formats, and behavioral contracts will continue to evolve as they are validated; the README and documentation of each subproject are the primary references for its current status, build process, and compatibility scope.

## Where to Start

If you are interested in models, data processing, or training methods, start with PhonoP2C. If you are interested in continuous-pinyin segmentation and its scoring model, see Phono-PinyinSegment. If you want to integrate the models into an application or explore on-device inference, start with phono-core.

## Contributing

Phono Project welcomes discussions, issues, and contributions related to Chinese phonetic input, language modeling, decoding algorithms, on-device inference, cross-platform integration, and documentation. Because the subprojects use different technology stacks and are at different stages of maturity, please read the relevant repository documentation first and open issues or changes in the most appropriate repository.

## Name

“Phono” draws from the ideas of sound, pronunciation, and phonological structure represented by phonetics and phonology. Here it does not restrict the project to speech recognition; it serves as a concise name for the broader direction of Chinese phonetic input.
