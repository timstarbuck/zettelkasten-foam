# Azure AI/Prepare to develop AI solutions on Azure

Artifical Intelligence
* Visual perception - The ability to use computer vision capabilities to accept, interpret, and process input from images, video streams, and live cameras.
* Text analysis and conversation - The ability to use natural language processing (NLP) to not only "read", but also generate realistic responses and extract semantic meaning from text.
* Speech - The ability to recognize speech as input and synthesize spoken output. The combination of speech capabilities together with the ability to apply NLP analysis of text enables a form of human-compute interaction that's become known as conversational AI, in which users can interact with AI agents (usually referred to as bots) in much the same way they would with another human.
* Decision making - The ability to use past experience and learned correlations to assess situations and take appropriate actions. For example, recognizing anomalies in sensor readings and taking automated action to prevent failure or system damage.

Data science 
: a discipline that focuses on the processing and analysis of data; applying statistical techniques to uncover and visualize relationships and patterns in the data, and defining experimental models that help explore those patterns.

Machine learning 
: a subset of data science that deals with the training and validation of predictive models. Typically, a data scientist prepares the data and then uses it to train a model based on an algorithm that exploits the relationships between the features in the data to predict values for unknown labels.

Artificial intelligence 
: usually (but not always) builds on machine learning to create software that emulates one or more characteristics of human intelligence.



**Understand considerations for responsible AI**
Fairness 
: AI systems should treat all people fairly. For example, suppose you create a machine learning model to support a loan approval application for a bank. The model should make predictions of whether or not the loan should be approved without incorporating any bias based on gender, ethnicity, or other factors that might result in an unfair advantage or disadvantage to specific groups of applicants.

Reliability and safety
: AI systems should perform reliably and safely. For example, consider an AI-based software system for an autonomous vehicle; or a machine learning model that diagnoses patient symptoms and recommends prescriptions. Unreliability in these kinds of system can result in substantial risk to human life. Rigorous testing and deployment management processes should be used.

Privacy and security
: AI systems should be secure and respect privacy. The machine learning models on which AI systems are based rely on large volumes of data, which may contain personal details that must be kept private. Even after models are trained and the system is in production, they use new data to make predictions or take action that may be subject to privacy or security concerns; so appropriate safeguards to protect data and customer content must be implemented.

Inclusiveness
: AI systems should empower everyone and engage people. AI should bring benefits to all parts of society, regardless of physical ability, gender, sexual orientation, ethnicity, or other factors.
One way to optimize for inclusiveness is to ensure that the design, development, and testing of your application includes input from as diverse a group of people as possible.

Transparency
: AI systems should be understandable. Users should be made fully aware of the purpose of the system, how it works, and what limitations may be expected.

Accountability
: People should be accountable for AI systems. Although many AI systems seem to operate autonomously, ultimately it's the responsibility of the developers who trained and validated the models they use, and defined the logic that bases decisions on model predictions to ensure that the overall system meets responsibility requirements. To help meet this goal, designers and developers of AI-based solution should work within a framework of governance and organizational principles that ensure the solution meets ethical and legal standards that are clearly defined.

**Capabilities**

Data scientists can use Azure Machine Learning throughout the entire machine learning lifecycle to:

* Ingest and prepare data.
* Run experiments to explore data and train predictive models.
* Deploy and manage trained models as web services.

Software engineers may interact with Azure Machine Learning in the following ways:

* Using Automated Machine Learning or Azure Machine Learning designer to train machine learning models and deploy them as services that can be integrated into AI-enabled applications.
* Collaborating with data scientists to deploy models based on common frameworks such as Scikit-Learn, PyTorch, and TensorFlow as web services, and consume them in applications.
* Using Azure Machine Learning SDKs or command-line interface (CLI) scripts to orchestrate DevOps processes that manage versioning, deployment, and testing of machine learning models as part of an overall application delivery solution.
