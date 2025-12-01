<img width="1080" height="606" alt="image" src="https://github.com/user-attachments/assets/e9bfabfd-984a-4da9-943e-e0773f60228f" />

### Problem Statement
The pace of AI innovation has accelerated dramatically. As Sundar Pichai recently noted, in a couple of years, businesses can end up building what they built in decades. Every day brings a new AI product release, expanding the ecosystem at an overwhelming speed. While this abundance is valuable, it also exposes a significant gap. Users have highly specific, ever-changing needs shaped by their work, environment, and workflows. No single AI tool or even a massive catalogue of them can cover the infinite variety of user requirements. In addition, this rapid acceleration creates a "paradox of choice."

In such a fast-moving landscape, individuals and organisations must be able to create AI agents tailored to their exact tasks. Custom, task-specific agents are becoming essential for effective work, reliable automation, and sustained productivity.

### Solution Statement
AI agents, equipped with strong reasoning, planning, and code-generation capabilities, can now build sophisticated tools on their own. With proper flow of instruction and management, they can strategize the workflow, develop the code and deploy a functional agent. However, this process must remain user-guided, making human–agent collaboration non-negotiable. This creates the need for an interface where users serve as architects while agents handle execution.

36Labs fills this gap by providing a unified master agent that builds custom tools or agents directly from user requirements. It simplifies multi-agent coordination and empowers anyone to create task-specific AI agents with ease. Though a few products in the market attempt to serve this purpose, 36Labs aims to offer a more versatile and user-friendly interface for effective collaboration.

### Overall architecture
To keep 36Labs intuitive for users, its architecture is designed to be simple and transparent, with each sub-agent named directly after its function. At the core are two key components: the **manager_agent**, which serves as the root agent of the system, and the **workflow_agent**, which acts as the communication bridge between the user and the internal agent structure. This design, built using Google’s ADK package, ensures clarity in how decisions are made, offering full visibility into the system’s operations rather than a traditional black-box experience. A clear breakdown of the architecture is demonstrated in the above flow chart.

* `questionarre_agent`: Every product begins with an idea or more specifically, a question. The user first interacts with the questionnaire_agent to express their intent, such as “_I want to create an agent for blog writing_”. Using Google’s built-in search tool, this agent gathers context and asks targeted (particular number of) questions to clarify the user’s needs. For the earlier instance, it might ask about the purpose, tone, communication style, and more. This structured inquiry helps the user articulate precise requirements, forming the foundation for all subsequent stages in the agent’s architecture.

* `workflow_agent`: Implemented as a **SequentialAgent**, it comprises two sub-agents: `memory_agent` and `strategy_agent`. The memory_agent takes the user’s clarified requirements and converts them into a structured dictionary, _{user_data_store}_, using predefined custom tools. This organised data enables the strategy_agent to design an effective outline for building the requested tool or agent. It then produces a second dictionary, _{workflow}_, which captures the complete plan. This structured flow of information ensures that the manager_agent receives clear, actionable instructions to execute and build the final product.

As the root agent of 36Labs, the **`manager_agent`** is a perfect example of a multi-agent system. This agent is responsible for developing the code, testing and refining it, until the final product is achieved, thereby managing the entire workflow. Implemented as a **SequentialAgent**, it consists of two key components: a **developer_agent** and a LoopAgent, **test_debug_agent**.

* `developer_agent`:  As the name suggests, the developer_agent is responsible for generating the initial draft of the code. For every sequential task defined in the _{workflow dictionary}_, this agent is invoked to produce the corresponding preliminary implementation. It operates under clearly defined guidelines to ensure consistency in structure, formatting, and tool usage within Google’s ADK framework. Once generated, each draft is stored in a dedicated dictionary, _{code_store}_, using custom tools designed for safe and organised code handling. Within the dictionary, the keys represent individual workflow steps and the values contain the code produced for each step. Since errors are inevitable, the developer_agent’s output is treated as a working draft rather than a final solution. Its role is to create a solid starting point that can be thoroughly inspected, tested, and refined in the subsequent debugging stages.

* `test_debug_agent`: Implemented as a LoopAgent, the test_debug_agent consists of two sub-agents: tester_agent and debug_agent. 

1. `tester_agent`: Receives the draft code and focuses solely on identifying issues rather than rewriting it. Error detection can be performed either through Gemini models or through custom-defined functions. 36Labs includes a built-in tool, **analyze_code**, which highlights potential errors in structure, format, or logic. These diagnostic functions must be updated regularly to recognise new error patterns effectively. All detected issues are then recorded in an _{error_store dictionary}_, where each entry corresponds to a specific workflow step. Once documented, the errors are passed to the debug_agent for further action. The tester_agent therefore just acts as critic, without altering the original code.

2. `debug_agent`: Based on the tester_agent’s feedback, the debug_agent rewrites only the specific sections of code that contain errors, preserving the overall structure and logic. After refining the required parts, it updates the _{code_store}_ dictionary with the corrected version.

The test_debug_agent repeats this process in a controlled loop for a predefined number of `max_iterations` steps, allowing gradual refinement without overcorrection. Once the loop completes, the final, improved version of the code is stored in _{code_store}_, ready for deployment. Finally, the `setup_agent` compiles all code snippets into a single, cohesive script ready for deployment. This final version is saved locally in _{final_agent_store}_ and simultaneously uploaded to the memory bank for future reference or reuse.

### Tools and Models
Along with _google_search_, 36Labs employs a suite of custom tools, _record_user_answer, agent_outline, save_code, analyze_code, and save_final_agent_ , used by the memory_agent, strategy_agent, developer_agent (also by debug_agent), tester_agent, and setup_agent respectively to store their outputs in structured dictionaries. Each agent is also powered by the gemini-2.5-flash-lite model, enabling reliable reasoning, code generation, and task execution throughout the workflow.

## Capstone Project
### Project Journey
The initial days of the capstone were dedicated to brainstorming and refining the core concept behind 36Labs. The project draws inspiration from platforms like ElevenLabs and EmergentLabs, both of which enable users to build sophisticated AI-driven tools and products, hence named after them. Early development focused on creating single-role agents and evaluating their outputs, iteratively improving prompts and behaviours for consistency. As the project progressed, these isolated agents evolved into a well-structured multi-agent system capable of managing workflows, sessions, and collaborative reasoning. In later stages, A2A compatibility and long-term memory were integrated, greatly enhancing continuity and autonomy. Eventually, the fully developed system was deployed to the cloud, completing the journey from idea to functional platform.

### Track and Features

This project aligns best with the **Freestyle Track**, as its core concept is broad, flexible, and difficult to confine to a single category. 36Labs is inherently innovative, enabling users to create arbitrary AI tools and agents tailored to tackle real-world needs (Agents for Good), optimising organisational workflows (Enterprise Agents), or delivering highly personalised assistance (Concierge Agents).
The system incorporates multiple agentic features, including a coordinated multi-agent architecture, both built-in and custom tool integrations, robust session management, long-term memory, adherence to the A2A protocol, and end-to-end agent deployment.

## Installation and Setup

Kindly refer to the uploaded kaggle notebook file for the installation, setup and execution of the system

### Future Directions

While the current implementation of 36Labs is fully functional, the generated code still exhibits occasional formatting inconsistencies and import-related issues. Also, the present workflow_agent is structured to generate a basic outline in only three steps to create a relatively simple agent. Addressing this will require scaling the system to support parallel execution with multiple developer_agents, each guided by more detailed and structured prompts. Likewise, the tester_agents must evolve into stricter, more comprehensive evaluators equipped with an expanded library of error-detection rules.
In future, 36Labs can scale into a production-grade framework with the integration of MCP servers, observability and agent-evaluation pipelines, and improved cross-agent orchestration. With the state-of-the-art models such as Gemini 3.0, the platform can gain major enhancements in reliability and performance, enabling more precise, efficient, and fully autonomous agent creation.

### References

The links to the YouTube video and GitHub repository with thorough explanation of the project including the demos are attached to the writeup. The showcased demos run within minutes, that would typically take hours to code manually, thereby clearly illustrating the dramatic reduction in development time achieved through 36Labs.

### Acknowledgments
The author gratefully acknowledges all organisers and tutors of the Google × Kaggle 5-Day AI Agents Intensive course. The programme was exceptionally well designed, enabling professionals, students, and even non-traditional coders to understand core agentic concepts and confidently build their own AI agents. The author highly recommends this course for beginners entering the world of Agentic AI.
