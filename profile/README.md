# Phono Project

**面向中文音码输入法的开放基础设施。**

**Open infrastructure for Chinese phonetic input methods.**

Phono Project 致力于把中文输入从封闭的产品能力拆解为开放、可组合、可研究、可部署的基础组件：从连续拼音的音节解析，到上下文感知的汉字候选生成，再到移动端和嵌入式设备上的本地推理。

Phono Project aims to decompose Chinese input technology from a closed product capability into open, composable, researchable, and deployable building blocks—from syllable parsing for continuous pinyin, through context-aware Chinese character generation, to local inference on mobile and embedded devices.

项目当前以无声调全拼为主要实现路径，并以拼音音节作为各层之间的中间表示。这样的边界使全拼、双拼以及其他能够映射为拼音音节的输入方案，可以在未来共享分词、上下文建模、候选生成和端侧部署能力。

The current implementation focuses on tone-free full pinyin and uses pinyin syllables as the intermediate representation between layers. This boundary is intended to let full pinyin, double pinyin, and other schemes that can be mapped to pinyin syllables share segmentation, context modeling, candidate generation, and on-device deployment capabilities in the future.

> Phono Project 不是一个完整输入法产品，而是用于构建中文音码输入法的开放基础。

> Phono Project is not a complete input method product. It is an open foundation for building Chinese phonetic input methods.

## 为什么创建 Phono / Why Phono Exists

中文输入法连接着键盘编码、语言知识、用户上下文与终端系统，但这些核心能力通常被封装在完整产品之中。与此同时，公开的模型研究、数据处理方法和推理实现往往彼此割裂，距离一条可以真正集成、部署和持续演进的输入链路仍有不小的距离。

Chinese input methods connect keyboard encodings, linguistic knowledge, user context, and device systems, yet these core capabilities are usually enclosed within complete products. Meanwhile, open research on models, data processing, and inference is often fragmented, leaving a substantial gap between individual results and an input pipeline that can actually be integrated, deployed, and continuously improved.

Phono Project 希望缩短这段距离。我们将研究模型、确定性算法、运行时接口和端侧工程视为同一个问题的不同层面，并尝试为它们建立清晰、稳定且可以独立替换的边界。

Phono Project seeks to narrow that gap. We treat research models, deterministic algorithms, runtime interfaces, and on-device engineering as different layers of the same problem, and work to establish clear, stable, and independently replaceable boundaries between them.

## 设计原则 / Design Principles

**开放。** 模型、算法、接口与实现应当可以被阅读、验证和扩展，而不只作为不可见的产品能力存在。

**Open.** Models, algorithms, interfaces, and implementations should be available for study, verification, and extension instead of existing only as invisible product capabilities.

**可组合。** 音节解析、P2C 模型、候选解码与运行时应保持明确边界，使不同研究和应用可以选择、替换或复用其中的部分组件。

**Composable.** Syllable parsing, P2C modeling, candidate decoding, and runtime execution should retain clear boundaries so that research and applications can select, replace, or reuse individual components.

**端侧优先。** 本地推理不是完成云端原型后的附加适配，而是模型结构、缓存策略、量化和接口设计从一开始就需要考虑的核心场景。

**On-device first.** Local inference is not an afterthought added after a cloud prototype; it is a primary scenario that shapes model architecture, cache strategy, quantization, and interface design from the beginning.

**研究走向实践。** 一个方法不仅应当能够训练和评估，也应当能够导出、集成、测试，并进入真实的输入交互链路。

**Research into practice.** A method should not only be trainable and measurable; it should also be exportable, integrable, testable, and usable in a real input interaction pipeline.

## 项目组成 / Projects

### [PhonoP2C](https://github.com/phono-project/PhonoP2C)

PhonoP2C 是上下文感知的拼音转汉字研究项目。它使用两段式编码器—解码器架构，以“中文历史上下文 + 拼音音节”为输入，生成汉字候选，并覆盖语料预处理、模型训练、评估、ExecuTorch 导出和模型包构建流程。

PhonoP2C is a research project for context-aware pinyin-to-Chinese conversion. It uses a two-stage encoder-decoder architecture to generate Chinese character candidates from Chinese history and pinyin syllables, covering corpus preprocessing, model training, evaluation, ExecuTorch export, and model package assembly.

### [Phono-PinyinSegment](https://github.com/phono-project/Phono-PinyinSegment)

Phono-PinyinSegment 是轻量的拼音间隙评分模型。它为连写拼音中相邻字符之间的边界提供分数，再由合法拼音词表构成的 Trie DAG 完成全局约束解码，而不是对每个间隙独立地进行贪心判断。

Phono-PinyinSegment is a lightweight pinyin gap-scoring model. It scores boundaries between adjacent characters in continuous pinyin, while globally constrained decoding is performed over a Trie DAG induced by the legal pinyin vocabulary instead of making independent greedy decisions at each gap.

### [phono-core](https://github.com/phono-project/phono-core)

phono-core 是面向端侧部署的 C++ 推理引擎。它在 ExecuTorch 上组合拼音规范化、智能分词、合法路径解码、上下文与 KV Cache 管理、候选生成和 beam search，并通过稳定的 C ABI 向移动端、嵌入式设备及其他语言绑定提供能力。

phono-core is the C++ inference engine for on-device deployment. Built on ExecuTorch, it combines pinyin normalization, intelligent segmentation, legal-path decoding, context and KV-cache management, candidate generation, and beam search, exposing these capabilities to mobile, embedded, and language-binding integrations through a stable C ABI.

## 当前链路 / Current Pipeline

当前三个项目共同覆盖从模型研究到端侧运行的主要链路：连续拼音首先经过规范化与音节边界建模，在合法拼音词表的约束下得到音节序列；随后，P2C 模型结合已提交的中文上下文生成汉字候选；最终，C++ 运行时负责缓存、解码、上下文复用和应用集成。

Together, the three projects cover the main path from model research to on-device execution: continuous pinyin is first normalized and modeled for syllable boundaries, then decoded into a syllable sequence under the constraints of a legal pinyin vocabulary; the P2C model combines that sequence with committed Chinese context to generate character candidates; finally, the C++ runtime handles caching, decoding, context reuse, and application integration.

## 当前范围 / Current Scope

当前实现聚焦于无声调全拼、简体中文候选生成和端侧推理。双拼键位映射、完整输入法前端、用户词典、长期个性化以及跨设备同步等能力属于更大的项目愿景，但不应被理解为现阶段已经提供的开箱即用功能。

The current implementation focuses on tone-free full pinyin, Simplified Chinese candidate generation, and on-device inference. Double-pinyin key mapping, a complete input method frontend, user dictionaries, long-term personalization, and cross-device synchronization belong to the broader project vision, but should not be understood as ready-to-use features available today.

项目仍处于积极开发和研究阶段。接口、模型格式和行为约定会随着验证结果继续演进；各子项目的 README 与文档是了解其当前状态、构建方式和兼容范围的主要依据。

The project remains under active development and research. Interfaces, model formats, and behavioral contracts will continue to evolve as they are validated; the README and documentation of each subproject are the primary references for its current status, build process, and compatibility scope.

## 从哪里开始 / Where to Start

如果你关注模型、数据处理或训练方法，请从 PhonoP2C 开始；如果你关注连续拼音切分及其评分模型，请阅读 Phono-PinyinSegment；如果你希望把模型集成到终端应用或研究端侧推理，请从 phono-core 开始。

If you are interested in models, data processing, or training methods, start with PhonoP2C. If you are interested in continuous-pinyin segmentation and its scoring model, see Phono-PinyinSegment. If you want to integrate the models into an application or explore on-device inference, start with phono-core.

## 参与项目 / Contributing

Phono Project 欢迎围绕中文音码输入、语言建模、解码算法、端侧推理、跨平台集成和文档提出讨论、问题与贡献。由于各子项目的技术栈和成熟度不同，请在参与前阅读对应仓库的说明，并尽量在最相关的仓库中提交 issue 或变更。

Phono Project welcomes discussions, issues, and contributions related to Chinese phonetic input, language modeling, decoding algorithms, on-device inference, cross-platform integration, and documentation. Because the subprojects use different technology stacks and are at different stages of maturity, please read the relevant repository documentation first and open issues or changes in the most appropriate repository.

## 名称 / Name

“Phono”取自 phonetic 与 phonology 所代表的声音、读音和音系概念。它在这里不是对语音识别的限定，而是项目对中文音码输入这一更广阔方向的简称。

“Phono” draws from the ideas of sound, pronunciation, and phonological structure represented by phonetics and phonology. Here it does not restrict the project to speech recognition; it serves as a concise name for the broader direction of Chinese phonetic input.
