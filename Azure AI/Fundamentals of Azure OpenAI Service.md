# Azure AI/Fundamentals of Azure OpenAI Service

Capabilities of OpenAI AI Models


|Capability|	Examples|
|--|--|
|Generating natural language	|Such as: summarizing complex text for different reading levels, suggesting alternative wording for sentences, and much more|
|Generating code	|Such as: translating code from one programming language into another, identifying and troubleshooting bugs in code, and much more|
|Generating images	|Such as: generating images for publications from text descriptions and much more|

Generative AI 
: models can produce new content based on what is described in the input. The OpenAI models are a collection of generative AI models that can produce language, code, and images.

Azure OpenAI is available for Azure users and consists of four components:
* Pre-trained generative AI models
* Customization capabilities; the ability to fine-tune AI models with your own data
* Built-in tools to detect and mitigate harmful use cases so users can implement AI responsibly
* Enterprise-grade security with role-based access control (RBAC) and private networks

Azure OpenAI supports many generative AI workloads such as:
* Generating Natural Language
  * Text completion: generate and edit text
  * Embeddings: search, classify, and compare text
    * Embeddings is an Azure OpenAI model that converts text into numerical vectors for analysis. Embeddings can be used to search, classify, and compare sources of text for similarity.
* Generating Code: generate, edit, and explain code
* Generating Images: generate and edit images


Azure OpenAI models include:

* GPT-4 models that represent the latest generative models for natural language and code.
* GPT-3.5 models that can generate natural language and code responses based on prompts.
* Embeddings models that convert text to numeric vectors for analysis - for example comparing sources of text for similarity.
* DALL-E models that generate images based on natural language descriptions.


DALL-E capabilities
* **Image generation** - Use a text prompt, more detailed prompt can provide better results.
* **Editing an image** - uploading the original image and specifying a transparent mask that indicates what area of the image to edit. Along with the image and mask, a prompt indicating what is to be edited instructs the model to then generate the appropriate content to fill the area.
* **Image variations**- Image variations can be created by providing an image and specifying how many variations of the image you would like.

**Azure OpenAI's access and responsible AI policies**

* **Fairness**: AI systems shouldn't make decisions that discriminate against or support bias of a group or individual.
* **Reliability and Safety**: AI systems should respond safely to new situations and potential manipulation.
* **Privacy and Security**: AI systems should be secure and respect data privacy.
* **Inclusiveness**: AI systems should empower everyone and engage people.
* **Accountability**: People must be accountable for how AI systems operate.
* **Transparency**: AI systems should have explanations so users can understand how they're built and used.

