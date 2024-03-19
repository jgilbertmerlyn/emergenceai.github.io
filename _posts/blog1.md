# A survey of intelligent agent frameworks, from the 70s to today.
###### _6 minute read_. Written by Prasenjit Day, Ashish Jagmohan, and Ravi Kokku.

## Actors

In 1973, MIT computer scientist Carl Hewitt published the [paper](https://www.researchgate.net/publication/220812785_A_Universal_Modular_ACTOR_Formalism_for_Artificial_Intelligence), “A Universal Modular ACTOR Formalism for Artificial Intelligence.” In it, he defines an “actor” as an individual unit of computation which can perform tasks and pass along messages. An actor satisfies the requirements of: “If for each actor A, the intention of A is satisfied => that the intentions of all actors sent messages by A are satisfied.” Actors can perform tasks concurrently, fail independently, and communicate with one another. Like employees in an office, actors are programs within a distributed system whose functions may include performing, receiving, and delegating tasks simultaneously with other actors, without interfering with one another. 

<img src="https://github.com/EmergenceAI/EmergenceAI.github.io/blob/main/_posts/_images/fig24.png" width="500">

## Autonomous Agents

An _autonomous_ agent built to, say, play chess, might be capable of learning its opponents patterns, of teaching itself to get better at the game, and of devising complex long-form strategies. 

The formal definition of an autonomous agent was proposed in 1997 by Stan Franklin and Art Graesser, in a [paper](https://cse-robotics.engr.tamu.edu/dshell/cs631/papers/franklingraesser96agents.pdf) titled, “Is it an Agent, or just a Program?: A Taxonomy for Autonomous Agents.” Synthesizing various definitions from within their contemporary academy, they came to the notion that:

_An autonomous agent is a system situated within and a part of an environment that senses that_
_environment and acts on it, over time, in pursuit of its own agenda and so as to effect what it_
_senses in the future._

Examples of autonomous agents which _do not_ use large language models as their central intelligence engines might be self-driving cars or robot vacuums.

## LLM-Powered Autonomous Agents

In 2017, Google Brain published the [paper](https://proceedings.neurips.cc/paper_files/paper/2017/file/3f5ee243547dee91fbd053c1c4a845aa-Paper.pdf) “Attention is All You Need.” It introduced transformer architectures to the field of artificial intelligence, that would go on to be the foundational building blocks of large language models. LLMs represented a huge breakthrough in agents’ abilities to understand and generate natural language.

LLM-based autonomous agents combine the intelligence of an LLM with mechanisms for planning, long-term memory, and tool use. Using techniques such as [chain-of-thought prompting](https://arxiv.org/pdf/2201.11903.pdf), developers are able to obtain multi-step reasoning and the breaking down of tasks in LLMs.

Below is a review of some selected LLM-based autonomous agents that have recently changed the way we interact with AI and machines at large.

### [AutoGPT](https://github.com/Significant-Gravitas/AutoGPT)

AutoGPT was developed to help GPT-4 perform actions such as search the web, store information in long-term memory, write to files, and more. It acts as a goal-driven autonomous agent. Given a large-scale task, AutoGPT will identify necessary subtasks and perform them autonomously until the given goal is reached.

### [Task Driven Autonomous Agent (TDAA)](https://yoheinakajima.com/task-driven-autonomous-agent-utilizing-gpt-4-pinecone-and-langchain-for-diverse-applications/)

This framework, introduced by Yohei Nakajima, uses GPT-4 (Intelligence), PineCone (long term memory), and Langchain (orchestration) to autonomously complete goal-driven tasks. It successively reprioritizes tasks as they are completed. It is not clear what the applied standard is for full completion of a task.

### [BabyAGI](https://github.com/yoheinakajima/babyagi) 

BabyAGI is a simplified revision to TDAA. It runs in an infinite loop script and uses OpenAI models, along with Chroma or Weaviate, to create, prioritize, and execute tasks. It stores and retrieves queued tasks from vector databases. This framework makes for a great prototype, but it cannot be scaled to production systems. Like TDAA, BabyAGI lacks a formal understanding of what it means to reach a goal. Once engaged, it can go on in an infinite loop until it is stopped.

### [ShortGPT](https://github.com/RayVentura/ShortGPT) 

This agent automates the process of video creation and editing by controlling state-of-the-art video editing tools in succession. It uses OpenAI models, such as GPT-4, to write video scripts and to generate prompts for itself. ShortGPT then uses Moviepy to edit videos, ElevenLabs or Microsoft's free EdgeTTS for voice synthesis, and Bing Search for sourcing images.

## Multi-Agent Systems

As intelligent as the above autonomous agents may be, they, like human workers, can only focus on one objective at a time. What if we could apply Hewitt’s concept of independent actors, which work contemporaneously toward synergistic objectives, to autonomous agents? 

That framework, whereby multiple autonomous agents build on each other’s work concurrently, is called a multi-agent system. These types of systems allow for the accomplishment of much more complex tasks than those that can be achieved by a single autonomous agent. For example, a multi-agent system can design, build, and test a new computer program (entirely autonomously) by deploying several agents taking on different specialized roles involved in the software development lifecycle.

Read about multi-agent systems, and the future that may follow them, in our next blog post.
