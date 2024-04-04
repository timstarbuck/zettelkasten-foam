# Azure AI/Fundamentals of Text Analysis with the Language Service

Azure AI Language is a cloud-based service that includes features for understanding and analyzing text. Azure AI Language includes various features that support sentiment analysis, key phrase identification, text summarization, and conversational language understanding.

NLP
: Natural Language Processing

Text analysis
: NLP process which extract information from unstructured text.

Tokenization 
: breaks down a corpus of text into tokens. 

Tokenatization Concepts
* **Text Normalization** May remove puncuation and lower case all text. Can lose semantic meaning: "Mr Banks has worked in many banks"
* **Stop word removal** the, a, it - examples of stop words - don't add much semantic meaning.
* **n-grams** multi-term phrases such as "I have" "he walked", Can be unigrams, bi-grams, tri-grams. Grouped words can make more sense to the learning model.
* **Stemming** - words with same root can be interpreted as the same token. i.e. power, powered, powerful.

Frequency analysis
: Count the occurrences of each token. *Term frequency - inverse document frequency (TF-IDF)* score comparing how often a word occurs in one document vs how often it appears in a collection of documents. Higher occurence in one document vs others increases the relevancy of the term.

Semantic language models use *embeddings* - multi-valued arrays of numbers. These group tokens in a particular dimension, so relted words are grouped closer together.

Common NLP tasks supported by language models include:

* Text analysis, such as extracting key terms or identifying named entities in text.
* Sentiment analysis and opinion mining to categorize text as positive or negative.
* Machine translation, in which text is automatically translated from one language to another.
* Summarization, in which the main points of a large body of text are summarized.
* Conversational AI solutions such as bots or digital assistants in which the language model can interpret natural language input and return an appropriate response.

**Azure AI Language** features:

* **Named entity recognition** identifies people, places, events, and more. This feature can also be customized to extract custom categories.
* **Entity linking** identifies known entities together with a link to Wikipedia.
* **Personal identifying information (PII) detection** identifies personally sensitive information, including personal health information (PHI).
* **Language detection** identifies the language of the text and returns a language code such as "en" for English.
* **Sentiment analysis and opinion mining** identifies whether text is positive or negative.
* **Summarization** summarizes text by identifying the most important information.
* **Key phrase extraction** lists the main concepts from unstructured text.

Azure AI Language resources:
* **A Language resource** - choose this resource type if you only plan to use Azure AI Language services, or if you want to manage access and billing for the resource separately from other services.
* **An Azure AI services resource** - choose this resource type if you plan to use Azure AI Language in combination with other Azure AI services, and you want to manage access and billing for these services together.
