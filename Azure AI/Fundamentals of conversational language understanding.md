# Azure AI/Fundamentals of conversational language understanding

Three core concepts: *utterances*, *entities* and *intents*.

Utterances
: An example of something a user might say and must be interperted. *"Switch the fan on." "Turn the light on."*

Entities
: An item which a utterance refers. For example *fan/light*.

Intents
: Purpose or goal expressed in a user's utterance. A conversational language understanding app defines a model consisting of intents and entities. Utterances are used to train the model to identify the most likely intent and the entities to which it should be applied.


Resources:
* **Azure AI Language**: A resource that enables you to build apps with industry-leading natural language understanding capabilities without machine learning expertise. You can use a language resource for *authoring and prediction*.
* **Azure AI services**: A general resource that includes conversational language understanding along with many other Azure AI services. *You can only use this type of resource for prediction.*

**Authoring**
Create a authorizing resource.
Define entities and intents your app will predict as well as utterances for each intent used to train the model. 
Can use prebuilt domains which include pre-defined intents and entities for common use cases as a starting point, can also create your own.
Easiest to use Language Studio - can also write code to define the elements of the model.

**Training the model**
Use sample utterances to teach the model to match natural language expressions. 
After training, test by submitting and reviewing the predicted intents. This is an iterative process.

**Predicting**
After successful testing/training, publish the Conversation Language Understanding app to a prediction resource for consumtion. 
Client apps use the model by connecting to the endpoint and specifying the appropriate auth key, submit user input and get predicetd intents and entities.


