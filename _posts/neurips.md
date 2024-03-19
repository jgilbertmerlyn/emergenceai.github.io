###### _4 minute read._
# Emergence’s automated mathematical graphing, as [presented at NeurIPS 2023 conference](https://gaied.org/neurips2023/files/39/39_paper.pdf)

At Emergence, we are creating purpose-built AI solutions for the automation of human workflows in real-world industries.

We believe that the next significant advancement in workflow automation will come from the planning, selection, and use of multiple external tools by artificial intelligence. Appropriate use of tools requires goal-oriented chains of thought reasoning and the decomposition of tasks into efficiently executable steps.  

Our classroom digital assistant helps to increase teacher productivity by enabling educators to control technology multi-modally: with voice, touch, and remote control. By operating external tools based on natural language commands, the assistant aims to free teachers from their desktops, allowing them to focus on the human side of education.  

Notably, this assistant autonomously coordinates the use of multiple tools at a time. An application of this ability was showcased [in a paper, titled “An Automated Graphing System for Mathematical Pedagogy,”](https://gaied.org/neurips2023/files/39/39_paper.pdf) written by our team at Emergence. The paper, which was chosen to be presented at an upcoming workshop at the 2023 Conference on Neural Information Processing Systems (NeurIPS 2023), describes a graphing functionality invoked by natural language.

Imagine a classroom in which a teacher could say to a digital assistant, “Graph a parabola that I can manipulate the coefficients of,” or, “Draw two concentric circles and tangents to each that are parallel to the y-axis," leading the assistant to instantly bring up these graphs using a variety of tools. Our framework achieves this with a combination of an LLM, a graphing calculator (Desmos), and a set of math problem solvers (including Wolfram Alpha). This automated graphing system takes in utterances, uses the LLM to break the speaker’s intent into multiple steps, uses the solvers as needed at each step, converts the solutions into mathematical expressions, and graphs them with Desmos.  

## Overview of LLM+Solver automated graphing system

<img src="https://github.com/EmergenceAI/EmergenceAI.github.io/blob/main/_posts/_images/neurips_chart.png" width="500">

In the above flowchart featured in the paper, an overview is given of the automated graphing system. A teacher can input (by speaking) an unformatted prompt in natural language, such as, “Graph y equals x cubed minus 6 x squared plus 9 x plus 4 and find the relative extrema.” The LLM will rephrase the question into a query understandable by a solver like Wolfram Alpha, which will then provide the solution. The LLM will then take that solution, write a detailed explanation of it, and use it to create expressions which can be understood by Desmos. A graph is produced and provided alongside the LLM’s explanation.

A hurdle which had to be overcome was the evaluation of the accuracy of output mathematical statements. AI systems may be able to evaluate equivalence for simple expressions, but their judgment becomes inconsistent for more complex ones. Our team’s paper notes that lexical similarity metrics used in natural language processing might consider “5=2+3” to be more similar to “5=2+4” than to “5=4+1” due to the greater number of characters it shares with the former. Therefore, evaluating the correctness of the outputs of the above graphing system required a technical solution of its own.

We used SymPy, a computer algebra system, to evaluate the expressions output by the LLM in the above system. SymPy could accurately isolate variables and check math statements, but it could not parse the inconsistently formatted equations which LLMs are known to output. In the case of SymPy failure, a backup LLM-only evaluator was implemented.

By running the LLM+SymPy (with LLM-only backup) auto-evaluation tool and an LLM-only auto-evaluation tool on a given dataset, checking the results against manual evaluation, we were able to see a significant increase in mathematical accuracy when using the LLM+SymPy solution.  
