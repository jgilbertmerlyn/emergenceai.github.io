# Self-improving agents

By definition, the fundamental characteristic of an agent is its intelligence in taking actions, checking outcomes against those actions, and improving itself if those actions result in divergence from its goals. Self-improvement, where an agent autonomously improves its own functioning, has intrigued the AI community for several decades.

We believe that the creation of agents that can autonomously adapt and improve is critical to the building of robust agent frameworks that are scalable and maintainable. One way we qualitatively think about self-improvement is by attempting to make the following distinction between “narrow” and “broad” self-improvement:

- **Narrow self-improvement**: This category contains improvements upon agent performance within the context of a fixed operating environment or goal. A simple example of this would be an LLM-based agent that monitors its own performance and can autonomously kick off a fine-tuning loop to retrain its LLM on a new dataset when it evaluates its own performance as deviating from within an acceptable range. Such agents can adapt, for example, to data distribution mismatches without fundamental changes in agent architecture, training algorithm, etc.

- **Broad self-improvement**: This category encompasses broader and more sophisticated modalities including agents that can create tools and agents that can modify their own architecture and even create new agents. The latter has sometimes been termed recursive self-improvement, and has been conjectured or feared to potentially enable “intelligence explosion” and/or “AI-takeoff” \[1, 2].

While these categories inevitably form a spectrum (depending on how broadly the environment is defined), they offer one structured way of classifying specific self-improvement approaches and problems. Let’s consider a few examples:

- Consider an agent that relies on progressive and adaptive data augmentation to self-improve. Such an augmentation loop may be quite sophisticated; it may involve the agent knowing when to invoke teacher models or humans to help with creating new data; it may also involve the agent autonomously reasoning about root-cause error analysis and about what types of new data to generate to improve its performance. Nevertheless, as long as the agent is restricted to optimizing for a given goal, this would be an example of narrow self-improvement.

- Consider an agent that uses a library of software functions to solve tasks. Consider also that the agent can add to this library by writing new code when needed. The agent plans to use a combination of these functions as required by the task or workflow that it is tasked with automating. Per the definition above, this would be an example of broad self-improvement.

- Now consider an agent that relies on data augmentation (as in our first example) but is not restricted to a single pre-specified task; thus the agent can seek to train itself on multiple tasks, including tasks that it has not encountered before. Classifying this type of meta-cognition is harder, and may ultimately depend upon the scope or range of tasks. Therefore, this would be a prime example of an agent whose self-improvement resides in between the two extremes in the spectrum.

## Building Narrow Self-improving Agents

In this section, we will focus on narrow self-improvement since we see immediate applications in improving efficiencies of human-agent workflows. Once the agents are given their goals (seen below, the “what” of the problem), a narrow self-improving agent monitors its own performance against those goals and autonomously kicks off a fine-tuning loop to retrain its LLM on a new dataset when it evaluates that its performance is deviating from an acceptable range.

The property of narrow self-improvement needs to be understood in the following framework:

1. **Why**: It is important to understand the purpose of a self-improvement run before defining what it should include. For example, the key intent of self-improvement could be just to meet the goal to an acceptable threshold provided by the programmer or to maximize its efficiency/value/incentives. This is the goal for a self-improvement run.

2. **What**: Once the agent knows what its goal is, it needs to understand what to improve. This is non-trivial, as the agent by itself may not know why its performance parameters are not where they should be. On the one hand, it could self-analyze (introspect) to understand which part of itself it should improve to get maximum return. More pragmatically, the improvement could be triggered by explicit feedback from a Feedback Agent (which by itself could be a combination of a human and an LLM-agent that is better trained for the task) or a Planning and Optimization Agent. Note that the more powerful the LLM that the feedback/planning agent uses, the more expensive it is to use.

3. **When**: An agent going into a self-improvement state too frequently is not a good policy, and neither is the policy of self-improvement where the agent waits too long to incorporate feedback, since both incur costs in their ways. There should be an optimal policy regarding when an agent decides to go into self-improvement, and there should be a mechanism to decide whether the agent goes out of operation or whether, alternatively, a clone agent continues the operation until the original agent comes back from self-improvement.

4. **How:** An agent must have the ability to decide what strategy to employ to improve itself. The framework for this decision consists of a Planning and Optimization Agent that provides a plan of action for the agent to improve itself given the goal and metrics of success. For example, if the plan requires the agent to get some good quality training samples or demonstrations from a Teacher Model or a Human Teacher, the framework sets up the various endpoints to get those samples from the model or human. Once the plan is satisfied, the Evaluation Agent checks against the metrics specified to see whether they were met. If the plan is not satisfied, the agent is sent back to the Planning and Optimization Agent to create a new plan. Similarly, a self-improvement plan could involve self-play of the agent to improve its capability. There too the agent would have well-defined metrics on when it can consider the self-improvement done.

Based on the above framework, the training or fine-tuning activities for self-improvement are done autonomously in the following steps:

- _Monitoring_: Gathering feedback about the performance of an existing model and benchmarking a new model using a combination of validation datasets created using teacher-LLMs or humans-in-the-loop.

- _Planning and Optimization_: Once an agent realizes that it needs to improve its performance, a Planning Agent helps it diagnose the best course of improvement for the original agent’s performance. It is possible that the performance of an LLM in an agent can be improved using SFT samples generated from a Teacher-LLM, or it could need human demonstration samples to improve its performance.

- _Data Generation_: The agent uses the plan and the diagnosis prepared by the planning agent to generate samples using one or more Teacher-LLMs or humans to create training or fine-tuning datasets that improve its LLM.

- _Training_: The agent deploys itself automatically on a training infrastructure, where the infrastructure assesses the agent’s configuration manifest to deploy it in the right infrastructure for training. The training scripts automatically run the training activity and create a new version of the LLM inside the agent.

- _Deployment_: Once the training metrics are validated, the agent deploys itself on the production infrastructure where it is monitored in an A/B testing scenario and replaces its previous clone once a performance metric is met.

Open AgentSelfImprovement.png

## A Note on Alignment

An important aspect of self-improving agents is the associated Alignment problem. AI Alignment refers to the problem of ensuring that AI systems produce outcomes that are consistent with human values and preferences. While there has been much well-known recent work on the alignment of “static,” non-self-improving agents, the problem of aligning self-improving agents is not only significantly harder, but even more critical.

As a thought experiment, consider a formalized notion of intelligence defined in \[3], where $\mu$ is the environment and $\pi$ is the agent policy, the intelligence $\Upsilon$ is a discounted product of the Kolmogorov complexity $K$ of the environment and value function $V$.

Open Screenshot 2024-03-13 at 3.44.39 PM.png

Briefly, the above argues for a universal prior over environments or goals ordered by complexity, with an agent’s intelligence measured by a weighted sum of performance over this set. Let’s consider how self-improvement and alignment (for some specification of human values, perhaps through a constitution-type enumeration) may fit into the above.

- Roughly, per the above, an intelligent agent should perform well in simple environments (which have higher probabilities based on shorter description lengths), and it should perform reasonably well compared to other agents in more complex environments. Therefore, self-improvement policy of agents should optimize for this behavior.

- The difficulty in evaluating the intelligence of a self-improving agent arises from estimating, or even defining, the value function $V$. With recursive improvement, where a first agent can create a second agent which in turn can create a third agent, which value function should be used to measure the first agent’s intelligence? Should the value function attained by the Nth agent be attributed to the first agent’s intelligence?

- How about alignment? Consider that the reward specification is a characteristic of an environment. Intuitively, it would appear that if we consider two identical environments, the first of which does not require alignment (with the provided constitution) while the second does, the second environment has higher complexity, and hence lower weight is given to an agent’s performance in the second. Thus, over the set of all environments, could an agent that does not care about alignment appear more intelligent than an agent that does care about alignment? Or can we build agents that have minimal alignment tax on the first type of environment?

- On the other hand, perhaps a better formulation is to consider alignment to be an imperative of the agent, as opposed to an environment reward specification. Consider alignment to a constitution as a sparse distribution in the state-action space $\sigma(a, s)$ where the rewards are $1s$ at spaces that are allowed and $0s$ at spaces that are not allowed or unspecified (depending on whether we are optimizing for false positives or false negatives in assigning rewards in the unspecified part of the alignment policy). Then the alignment is a modification of the reward function $V$ where a lower distance $D (\sigma, \pi)$ of policy $\pi$ to $\sigma$ is rewarded and deviation from the policy is penalized. This means that even though the environment may have its own reward function that the agent is trying to optimize for, the agent also has a penalty for misalignment to its constitution.

We propose a possible incorporation of misalignment in the intelligence formula above which we call aligned intelligence $\Sigma$. We assume a new value function $V\prime$ which incorporates the reward for being aligned to $\sigma$.

Open Screenshot 2024-03-13 at 3.34.56 PM.png

One possible modified reward function that uses $D$, the distance measure between the agent policy $\pi$ and alignment policy distribution $\sigma$, where larger distances (misalignment) are penalized could be as shown below where $R_i$ is an appropriate reward function defined to capture the distance of rewards between $\sigma$ and $\pi$. For example, a simple distance reward could be $2^-D(\sigma, \pi)$ where for small distances it goes to $1$ and for large distances it goes to $0$.

Open Screenshot 2024-03-14 at 2.42.23 PM.png

According to the expression above, a policy that provides the agent with large rewards from the environment but is not aligned with its constitutional imperatives will yield a lower overall value. Whereas, if both the alignment and environment rewards are in line, the (modified) value function can be large even though the environment reward may have been lower to start with.

 Open Screenshot 2024-03-14 at 12.34.56 PM.png
 
Self-improvement is one of the core aspects of an agent. By continuously learning, adapting, and optimizing their actions, these agents can help us make better decisions, streamline processes, and ultimately bring in huge productivity gains in human-agent workflows. However, it is important to approach the development and deployment of self-improving agents with caution, ensuring that ethical considerations, transparency, and accountability are prioritized. Since alignment and following a defined constitution by an agent could be a conflicting goal to continuous self-improvement, it is important to ensure the self-improvement loop is done within the boundaries of alignment. This is an extremely complex task and we will discuss this trade-off in our next blog.

#### References

1. [ Recursive Self-Improvement - LessWrong](https://www.lesswrong.com/tag/recursive-self-improvement)

2. [The Unavoidable Problem of Self-Improvement in AI: An Interview with Ramana Kumar, Part 1 - Future of Life Institute](https://futureoflife.org/ai/the-unavoidable-problem-of-self-improvement-in-ai-an-interview-with-ramana-kumar-part-1/)

3. Legg, S., & Hutter, M. (2007). Universal Intelligence: A Definition of Machine Intelligence. _Minds and Machines, 17_, 391-444.
