# Azure AI/Fundamentals of Generative AI

Artificial Intelligence (AI) imitates human behavior by using machine learning to interact with the environment and execute tasks without explicit directions on what to output.

Generative AI 
: describes a category of capabilities within AI that create original content. People typically interact with generative AI that has been built into chat applications. One popular example of such an application is ChatGPT, a chatbot created by OpenAI, an AI research company that partners closely with Microsoft.

Types: 
* Natural language generation
* Image generation
* Code generation

LLM
: Large Language Model - specialized type of machine learning model used to perform NLP (Natural Language Taska). 

**Transformer Models**
Today's LLMs are based on teh transformer architeture. Consist of two components or *blocks:*
* encoder block - creates a semantic representations of the training vocabulary
* decoder block - generates new language sequences

In practice, the specific implementations of the architecture vary – for example, the Bidirectional Encoder Representations from Transformers (BERT) model developed by Google to support their search engine uses only the encoder block, while the Generative Pretrained Transformer (GPT) model developed by OpenAI uses only the decoder block.

Tokenization 
: decompose training text into *tokens*, identify each unique value.

Embeddings
: contextual vectors. multi-values numeric representations of information [10, 3, 1] where each numeric element represent a particular attribute of the information. 
It can be useful to think of the elements in a token embedding vector as coordinates in multidimensional space, so that each token occupies a specific "location." The closer tokens are to one another along a particular dimension, the more semantically related they are. In other words, related words are grouped closer together. 

Attention
: technique used to examine a sequence of text tokens and try to quantify the strength of the relationships between them. In particular, self-attention involves considering how other tokens around one particular token influence that token's meaning. i.e. "the bark of a tree" and "I heard a dog bark" - bark has different meaning based on tokens around it. 


#### Azure OpenAI

Azure OpenAI Service is Microsoft's cloud solution for deploying, customizing, and hosting large language models.

Supported Models
* **GPT-4 models** are the latest generation of generative pretrained (GPT) models that can generate natural language and code completions based on natural language prompts.
* **GPT 3.5 models** can generate natural language and code completions based on natural language prompts. In particular, GPT-35-turbo models are optimized for chat-based interactions and work well in most generative AI scenarios.
* **Embeddings models** convert text into numeric vectors, and are useful in language analytics scenarios such as comparing text sources for similarities.
* **DALL-E models** are used to generate images based on natural language prompts. Currently, DALL-E models are in preview. DALL-E models aren't listed in the Azure OpenAI Studio interface and don't need to be explicitly deployed.

**Azure OpenAI Studio**
Azure OpenAI Studio, a web-based environment where AI professionals can deploy, test, and manage LLMs that support generative AI app development on Azure.


**Prompt Engineering**

System Messages
: sets the context for the model by describing expectations and constraints. i.e. "You're a helpful assistant that responds in a cheerful, friendly manner".

Writing good prompts
* use clear, specific prompts. “Create a list of 10 things to do in Edinburgh during August”.
* Provide examples. LLMs support *zero-shot* learning (no prior examples) providing *one-shot* learning prompts with one or more examples (“Visit the castle in the morning before the crowds arrive”.). Model can then generate further responses in the same style.
* Grounding data - include contextual data in the prompt so the model can use it to generate approriate output - include email text with an instruction to summarize it. 


