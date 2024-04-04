# Azure AI/Fundamentals of Azure AI Speech


Speech recognition
: the ability to detect and interpret spoken input

Models:
* An acoustic model that converts the audio signal into phonemes (representations of specific sounds).
* A language model that maps phonemes to words, usually using a statistical algorithm that predicts the most probable sequence of words based on the phonemes.

Speech synthesis
: the ability to generate spoken output

Resources:

* **A Speech resource** - choose this resource type if you only plan to use Azure AI Speech, or if you want to manage access and billing for the resource separately from other services.
* **An Azure AI services resource** - choose this resource type if you plan to use Azure AI Speech in combination with other Azure AI services, and you want to manage access and billing for these services together.

**Speech to Text API**
Universal Language Model - trained, owned by microsoft, deployed to azure. Optimized for conversational and dictation scenarios. Can create and train custom models including acoustics, language and pronunciation.

**Text to Speech API**
Incluses muliples pre-defined voices with support for multiple languages and regional pronunciation. 
