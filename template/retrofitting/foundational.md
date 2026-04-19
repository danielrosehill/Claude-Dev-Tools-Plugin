# Foundational Prompt - Broad Guidance

This project was forked or started from a code base that was developed by humans.

In the project's revised format, the project will be developed predominantly by agentic code generation tools (like you).

The process that will be followed can be described broadly as "human-guided agentic code development."

That is to say:

- The human user will assume responsibility and control for planning decisions and provide specific task instructions. 
- The human will supervise your operation and answer your questions as required. 
- The user will provide clear guidance to support the development of this project. 

In other words:

- This won't be autonomous agentic code generation. 
- But AI agents will take the lead in undertaking the actual edits. 

With that context in mind:

- Consider whether the current structure of the repository is optimally designed to support this use. 

Edits you may wish to undertake:

- Remove contributor guidelines. Do this unless the user informs you that the project will still be collaborative. If the user informs you that this will remain a collaborative project, but that contributors are expected or free to use their own agentic code generation tools, then you may wish to ask the user whether they want to provide any guidance as to how users may best do that. 
- Agentic coding disclosures: If the user informs you that the project will be collaborative or shared publicly (and only if) Then you may wish to add to the README a note to the effect that the development of the code was undertaken by AI agents. Do not assume that you will be the sole agent undertaking the development. But if the user indicates that you will, then you can credit yourself noting your specific model number. 

If you have a number of questions to ask the user, then you can use the question asking module in order to streamline the intake information. 

## Plan This Systematically 

You should approach the refactoring of the code base in a systematic manner:

- Firstly, look at the current state of the code base. 
- Next, consider broadly how this might be optimized for agentic-first code development. 
- Remember that AI agents thrive on specificity of purpose and modularization of tasks. As a foundational best practice, try to ensure that the code is well organized with clear divisions between folders. The folder names themselves should be descriptive of their purpose wherever possible. If you can, identify any points of potential ambiguity for an agent navigating the code base and try to resolve these to the extent that they will not introduce breaking changes to the code base. If the changes can be resolved by a quick refactor, then undertake them. 
- Artificial intelligence tools are very fast moving. Don't assume that the best practices that you have in mind from your own knowledge are up to date. Run a web search to ensure that your knowledge of best practices for agentic code generation are current. 

The suggested edits below are merely a starting point:

## Suggested Additions

- The user may wish to define a part of the repository for planning discussions and consultations and decisions. This will be separate from the code base. Always aim to ensure that the code is separated from other items in the project like this at the top level by a folder hierarchy. You can assume that the user wishes you to use a simple but descriptive folder name for the actual code part of the repository, such as "app" or "website." Choose the most appropriate one. Ensure that the name you choose will not break any deployment process, or if it might affect it, refactor the deployment code accordingly. 
- You may wish to add comments to various parts of the repository which are drafted specifically for easy intelligibility by AI agents navigating the repository. These comments might provide descriptions of key functionalities at various parts of the repository. 
- You should ensure that there is a CLAUDE.md which describes the overall purpose of the repository. 
- You should also ensure that there is a context folder which the user will use to gather context relevant to the repository, such as links to external documentation or API documents. If you can identify that MCP tooling is already used or available, make a note of the MCP tooling available and where it should be used. If it's not immediately clear from the context, ask the user to provide clarification.