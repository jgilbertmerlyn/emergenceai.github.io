---
layout: jekyll
title:   "My Post"
varA: default
---

# AutoWeb: Distilling the web for multi-agent automation<a id="autoweb-distilling-the-web-for-multi-agent-automation"></a>

## Introduction

Our everyday interaction with computers is filled with slow, tedious, and repetitive tasks and workflows. Automating these workflows would result in significant time savings and a reduced cognitive load for humans. With the advent of large language models (LLMs), we’ve seen the introduction of the possibility of creating systems that can perform more intelligent functions such as taking actions in the physical and digital world, planning language tasks in multi-agent settings, and following instructions in a collaborative manner. Agents driven by LLMs can make computing systems more easily blend into human systems, and they can lead to a spectrum of new collaborative paradigms, from blended systems to fully autonomous computing systems.

Among the regular day-to-day human-computer interactions, interactions with the web using keyboard and mouse represent a significant chunk. We now present a web interaction agent that can interact with a web page using natural language to complete a task. 

We have tested our agent for completion of tasks from e-commerce shopping to searching the web to filtering for vetted content on specific websites. We also tested it on form filling, question answering (“How much does X cost?”), and text extraction (“Tell me top soccer news.”). It is our belief that with a general-purpose LLM and a properly compacted/distilled HTML DOM, significant web-interaction automation can be achieved. We are soon going to release the code for this agent in the open-source community in the hope that the community will take it much further than we could by ourselves. We will provide the details of its GitHub repository and relevant demo videos in our subsequent announcements.

Our web agent architecture is built on ideas from Agent-Oriented Programming (AOP), where each agent encapsulates the characteristics shown in the figure below. An agent at the minimum contains four basic modules:

1. **Sensing**: It senses its environment.

2. **Processing**: It processes the sensed signal from the environment using qualities such as its intelligence driven by LLMs, its long-term memory that facilitates better processing, etc.

3. **Action**: It acts on its external environment through tools or code.

4. **Self-improvement**: It improves itself continuously to become better at a task. This part is closely connected to its need to stay aligned in the process of self-improvement dictated by its constitution.

For more details on the agent object model please refer to our previous blogs.

Open AgentObject.png

Agent Definition

## Description

Our agent builds on a scalable notion of atomic actions which we call “skills,” whereby the agent learns to use skills as building blocks to complete a large complex workflow, as opposed to trying to learn the whole workflow as one construct. This allows our agent to scale easily toward learning more tasks, as the atomic actions that allow manipulation of a web page do not change from one task to another. What does change is how these atomic actions are stitched together for different tasks. Our agent’s intelligence layer, driven by an LLM, learns this from one task to the next and helps generalize by learning how to stitch these atomic actions together.

Architecture of a Web Agent Interacting with Human Inputs and Webpage Execution

## Skills/Atomic Actions

To select the skills, we looked at how humans use the web and drew on that knowledge to put together a first set of skills to build our web agent. The skills, once implemented, have the agent perform a task and then return natural language text explaining the output. Making the skills focus on more conversational outputs yielded better results than a boolean true/false. The natural language response also helped the LLM figure out how to course-correct when errors occurred. Skills fall into two categories:

- _Sensing skills_: The skills that help the agent understand the state of the webpage or the browser.

- _Action skills_: The skills that allow the agent to act on the web page or browser which are components of its environment.

|                                                                                                                                                                                                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                    |
| :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|                                                                                                                                                                                                                      **Sensing Skills**                                                                                                                                                                                                                      |                                                                                                                                                                                          **Action Skills**                                                                                                                                                                                         |
|                                                                                                                                                                                                        _geturl_ - Fetches and returns the current url.                                                                                                                                                                                                       |                                                                                                                                                                    _click_ - given a DOM query selector, this will click on it.                                                                                                                                                                    |
| _get\_dom\_with\_content\_type_ - Retrieves the HTML DOM of the active page based on the specified content type. Content type can be: <br/><br/> **_'text\_only'_:** Extracts the inner text of the html DOM. Responds with text output. <br/> **_'input\_fields'_:** Extracts the interactive elements in the DOM (button, input, textarea, etc.) and responds with a compact JSON object. <br/> **_'all\_fields'_:** Extracts all the fields in the DOM and responds with a compact JSON object. | _enter\_text\_and\_click_ - Optimized method that combines enter text and click skills. The optimization here helps use cases such as enter text in a field and press the search button. Since the DOM would not have changed or changes should be immaterial to this action, identifying both selectors for an input field and an actionable button can happen based on the same DOM examination. |
|                                                                                                                                                    _get\_user\_input_ - Provides the orchestrator with a mechanism to receive user feedback to disambiguate or seek clarity on fulfilling their response.                                                                                                                                                    |                                                                                                                                    _bulk\_enter\_text_ - Optimized method that wraps enter\_text method so that multiple text entries can be performed one shot.                                                                                                                                   |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                              |                                                                                                                                                        _enter\_text_ - Enters text in a field specified by the provided DOM query selector.                                                                                                                                                        |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                              |                                                                                                                                                                       _openurl_ - Opens the given URL in current or new tab.                                                                                                                                                                       |



## DOM Distillation

HTML DOMs can be so large that, often, LLMs cannot use them in their raw forms, since they do not fit in the LLM’s context window. The image below shows the number of tokens required to represent the raw HTML of popular websites.

Open image-20240314-133244.png

Further, the size of the DOM can also have implications on the speed of processing, cost, and the quality of reasoning by the LLM. Hence, DOM compaction/distillation is an important step of pre-processing to improve important language signals in the DOM before an LLM can process it.

We approach the DOM in the way that a human approaches a website. There is a lot of information presented to them, however, only a small portion of it is relevant to the task at hand. Humans effortlessly decide and extract the information relevant to their task from the webpage, while skipping the task irrelevant information. This active filtering of task-irrelevant content keeps getting better as the user gets more familiar with the webpage. For example, if a user is on an e-commerce site looking for lightning deals, the product search engine functionality might not be what the user is interested in, and, therefore, the user may visually block it using a phenomenon called banner-blindness (a.k.a., paying selective attention).

Given a task, the LLM decides the type of DOM content it needs. Unless the content type received indicates text-only content, the output of the distilled DOM is a hierarchical JSON document that can be sent to an LLM for predicate resolution, where a predicate is a step in the execution plan such as identifying the text search field on this page. (If the content type indicates text only, then all the text in the DOM is extracted as one big string without any HTML tags.)

       {

         "tag": "button",

         "aria-label": "Skip navigation",

         "mmid": "898",

         "description": "Skip navigation"

       },

       {

         "role": "combobox",

         "name": "search_query",

         "autocomplete": "list",

         "haspopup": "listbox",

         "value": "cat videos",

         "tag": "input",

         "tag_type": "text",

         "placeholder": "Search",

         "mmid": "919"

       }

(Above is a sample DOM representation from YouTube.)

## Planning & Reasoning

When the agent receives an utterance such as “Find ‘Nothing Phone 2’ on Amazon and sort the results by best seller”, the LLM in the agent prepares a plan of action to complete the task. If some information is incomplete, it can reason out that it needs more information from the user to be able to move forward with the plan. Before even preparing a plan and starting to sense the environment, the agent needs to first decide what kind of sensing skills to deploy for the utterance.

Sometimes ambiguities (there may be multiple variants of Nothing Phone 2 with various specifications as well as Amazon’s rating of the product based on user feedback) and unexpected challenges (like a missing search box in the website or a site that does not work as expected) may arise during execution. The LLM can adapt the plan based on new information or prompt the user for further clarification if needed such as “Would you like me to sort the phones based on user rating or price?”

## Extensible Architecture

Our web agent architecture is highly scalable and extensible. More skills can be added easily by registering them using the AutoGen framework. Once that is done, the agent creates a JSON representation of the skills to make use of LLMs that support function-calling. The verbose representation of skills, while advantageous, can consume a significant number of tokens. We are considering using a lightweight router that can provide a compact representation of the available skills to the LLM while exposing itself as a skill so that it maintains the skill registration and extension paradigm.

How to precisely represent the state of the world, in this case the state of the DOM, is an area of active exploration. The smaller the DOM slice shared with the LLM, the faster and cheaper the response will be. This might be achievable by adding other content types or better distillation techniques. A compact representation of the state will allow for smaller LLMs that are hosted locally to perform these tasks which could boost privacy and adoption.

## Evaluation

Our evaluation of the agent is inspired by WebArena \[1]. However, Web Arena performs its evaluation against static, curated, and self-hosted sites. However, we are using the wild web and approximating the evaluation. We are looking at ways to use a modified version of Web Arena to objectively evaluate our web agent.

## Conclusion

In conclusion, we believe that highly capable web agents may be the path to extensive automation and perhaps lead toward general task-performing intelligence capabilities since a big part of our digital world including the apps and tools are web-based. However, we also believe that these generally capable web agents will coexist with agents that are highly specialized on a very complex task that requires extreme accuracy and is mission critical. This presents a huge opportunity for enterprise workflow automation where the tools are increasingly web-based and the workflows are task-specific which lends itself to using a configurable agent architecture for automation.

## References

\[1] Zhou, S., Xu, F. F., Zhu, H., Zhou, X., Lo, R., Sridhar, A., ... & Neubig, G. (2023). Webarena: A realistic web environment for building autonomous agents. _arXiv preprint arXiv:2307.13854_.
