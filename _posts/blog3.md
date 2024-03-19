# AI capabilities which remain necessary for agent frameworks.
###### Written by Prasenjit Day, Ashish Jagmohan, and Ravi Kokku. 

## What are agent frameworks?

An agent framework is a foundational structure by which a user can deploy autonomous agents. It might define a set of functions or states which all created agents employ. It might delineate how these agents interact with one another, set priorities, and achieve goals. A useful example might be _AutoGen,_ discussed in another of our blog posts, which provides a framework for communication between agents collaborating toward achieving a task.

## What foundational AI capabilities are required to realize paradigm-shifting agent frameworks?

To realize the vision of a truly game-changing agent framework, we will need AI systems that can do all of:

- Perform multi-step reasoning and sequential decision-making, remaining responsive to a changing environment state.

- Independently leverage software tools and APIs, within these multi-step plans.

- Continuously learn via demonstration, proactive self-play, and autodidacticism.

The recent successes in the field of large language models (LLMs) have enabled progress toward fulfillment of many of the above requirements. 

## Recent advancements in AI

### Multi-step reasoning

In [chain-of-thought (CoT) prompting](https://arxiv.org/pdf/2201.11903.pdf), an LLM solves a problem by generating a step-by-step rationale to arrive at a solution, somewhat mimicking a human’s sequence of problem-solving thoughts. 

LLMs are "auto-regressive." This means that their decoders generate text one piece at a time and use each new piece of text as additional context for generating the next. 

CoT, effectively, favorably modifies the conditioning context (prompt) used by the auto-regressive LLM decoder, like so:

![](https://lh7-us.googleusercontent.com/JEpvvBe0zGlSaI2SOB8PAmOgR3iGTrh61yAa_-l82H_H9j1_cMTg0MOI7qkc_v3k5PYxD0ruYh4XNavBEQFFvZENo0tRJMq3HnEMt2e5AcYqP6q6pchQaV8ajcQMicHM27SI-jvkehgWpKwpsKoWW5o)

Popular due to its efficacy, [multiple variants have been proposed](https://arxiv.org/pdf/2310.04959.pdf) and have shown significant gains on mathematical problems, multi-step question-answering, and commonsense and symbolic reasoning tasks. 

These also include approaches like [Tree-of-Thoughts](https://arxiv.org/pdf/2305.10601.pdf) that combine a process of search with chain-of-thought type step-by-step reasoning. 

While initially considered an emergent ability in large models like [Palm](https://arxiv.org/pdf/2204.02311.pdf), subsequent work has used techniques like distillation to [specialize smaller models](https://arxiv.org/pdf/2301.12726.pdf) for [specific reasoning tasks](https://arxiv.org/pdf/2306.11644.pdf).  

### The use of LLMs for self-critique

CoT-style step-by-step reasoning can be further combined with [self-critique driven refinement](https://arxiv.org/pdf/2303.17651.pdf), where an [LLM critiques its response](https://arxiv.org/pdf/2212.08073.pdf) (or a step in the reasoning), and then generates a subsequent refinement based on the critique.

### Sequential decision making

This capability involves an agent tracking a world-state that changes in response to the agent’s previous decisions/actions. The agent remains aware of this world-state in each decision.

Projects like [ReAct](https://openreview.net/pdf?id=WE_vluYUL-X) and [Reflexion](https://arxiv.org/pdf/2303.11366.pdf) demonstrate the use of LLMs for sequential decision-making. These agents intake feedback from their environments to self-critique and refine their trajectories _while_ working toward accomplishing a goal.

### Tool and API usage via AI

The text-generation capabilities of LLMs can be used to route queries to external tools and APIs, [enabling the fulfillment of more complex requests](https://arxiv.org/pdf/2205.00445.pdf) than could be handled by the LLM in isolation. 

The last year or so has seen several proposed AI models that are trained to use tools and APIs:

Models like [NL2API](https://ieeexplore.ieee.org/document/8456354), [ToolkenGPT](https://arxiv.org/pdf/2305.11554.pdf), [Toolformer](https://arxiv.org/pdf/2302.04761.pdf), and [Gorilla](https://arxiv.org/pdf/2305.15334.pdf) employ natural language processing to interact with parameterized APIs. These systems are trained on datasets that include examples of API usage, and they use retrieval-augmentation against API documentation for effective API selection.

### Continuous learning and self-learning

Finally, recent works including [Voyager](https://arxiv.org/pdf/2305.16291.pdf) and [Jarvis-1](https://arxiv.org/pdf/2311.05997.pdf) explore building agents that continuously learn via proactive world-exploration, building upon previously acquired skills. Both use Minecraft as their demonstration environment of choice.

![](https://lh7-us.googleusercontent.com/8yIfxmA_tZ2X4Hc43qPe_W7zF5P8ZoHHtcqTTbKyi5WvVOnqichDDjYWjubAp5ebyKFaMm3PpXyVYxIeXUYhGE4mIb_615Us3bObnYLI7m1Vp6C-G9Qi_Pzxt9O9tvlQ-CNFWQz9hskQlOfJ9F6W3hU)

Voyager, visualized above, uses curriculum learning, where the agent seeks to progressively learn harder skills by leveraging previously acquired skills. A “skill” is a multi-step plan (represented as code) to accomplish a discrete task (like “craft a stone sword”).

![](https://lh7-us.googleusercontent.com/6aNBl37vhcqGwJlaUOMVOKmSc1OnIoQp3zbsqi8k950PbT4lYWjAMHZoC3zboKsyJei_DIkW86OC95KsopMzcay3Era93VlTFCBNtTn2JEpGU3c48v5Au1uHRSX93T8V4JhlL0rwRNgPnV9oHYRCveM)

Jarvis-1, visualized above, performs self-learning via self-instructed curriculum generation, and (like Voyager) it uses self-critique and environmental feedback for long-horizon planning. It collects multimodal experiences in memory to assist with future planning.

## Relevant limitations in current AI.

While there has been significant progress in using language-models for reasoning, planning and tool-use, there remain gaps in capabilities:

- Limited planning capabilities, especially on tasks for which pre-training data was limited. In general, LLMs can often create loosely-described high-level plans, but they struggle at creating valid, executable plans. 

- Self-critique often does not produce significant improvements in practice. [Recent](https://arxiv.org/pdf/2206.10498.pdf) [works](https://arxiv.org/pdf/2310.01798) have shown that, in many scenarios, self-critique can even worsen the quality of an LLM’s planning and reasoning. 

- The [tendency](https://arxiv.org/pdf/2305.14552.pdf) of LLMs to [hallucinate](https://arxiv.org/pdf/2311.05232.pdf), which is impacted by [pre-training data mixes and fact frequencies](https://arxiv.org/pdf/2305.13169). 

Due to these and other limitations, current LLMs struggle to pass several existing automation benchmarks including [WebArena](https://arxiv.org/pdf/2307.13854.pdf) (on which even GPT4 has only 15% accuracy), [GAIA](https://arxiv.org/pdf/2311.12983.pdf), [AgentBench](https://arxiv.org/pdf/2308.03688), and others.

## Some potential strategies for mitigating gaps.

### Carefully constraining the domain and scope of agents

For example, robust tool and API usage may be easier to accomplish by training agents on tightly constrained sets of domains APIs. An API space whose cardinality is reduced by an order of magnitude or more (than that of a system like [Gorilla](https://arxiv.org/pdf/2305.15334.pdf)) should be significantly easier to train an LLM on.

### The design of high-quality task-specific data

This could significantly improve the performance of small models on planning, multi-step reasoning and tool invocation tasks. 

Consider the [phi models](https://arxiv.org/pdf/2306.11644.pdf); phi-2, for example, is a 2.7B parameter model, pre-trained on high quality data sets, that outperforms much larger models on multi-step reasoning tasks. 

It may be crucial to complex agent frameworks that small models can be used by constituent agents. In a network where an appreciable fraction of interactions are intermediated by AI, the cost of a GPT-4-type model is likely to be prohibitive.

### The use of sequential-decision making algorithms 

The use of reinforcement learning with human or AI feedback has gained [much prominence](https://arxiv.org/pdf/2203.02155.pdf) over [the last year](https://arxiv.org/pdf/2212.08073.pdf). 

Recent work has shown how "process" reward models—those where rewards are assigned based on solution-steps (rather than just the solution-outcome)—can [significantly improve reasoning performance](https://arxiv.org/pdf/2305.20050.pdf) for tasks other than alignment. 

Other [recent work](https://arxiv.org/pdf/2310.12931.pdf) has looked at [automated generation of reward signals](https://arxiv.org/pdf/2310.12921.pdf). 

### Better means of sensing environment state 

Understanding its environment can be complex for an agent. The difficulty of web automation is at least partly due to the imperfect accuracy in parsing web-page document object models (DOMs) and/or using vision to interpret and find elements on a page. 

Web-page DOMs can be arbitrarily complex, especially given that JavaScript code can modify a DOM when certain actions are taken on the page. Similarly, sensing via vision requires an understanding of the relationships between textual and visual elements as well as an ability to recognize potentially non-standard iconography. 

We will argue in the next post in this series that it may make sense to revisit such interfaces for agents. For example, it may make sense to imagine an entirely new web designed for AI interactions, in which case DOM parsing and vision are replaced by API manifests with which agents are trained to work directly.
