# Azure AI/Fundamentals of Responsible Generative AI

Four stages to develop and implement a plan for responsible AI when using generative models:

1. *Identify* potential harms.
2. *Measure* the presens of the harms in the outputs.
3. *Mitigate* the harms at multiple layers to minimize impact, ensure transparent communicate about potential risks to users.
4. *Operate* the solution responsibly by defining and following a deployment and operational readiness plan.


**Identify potential harms**
Steps
1. **Identify potential harms**
   1. Common Types of harms
      1. Generating content that is offensive, pejorative, or discriminatory.
      2. Generating content that contains factual inaccuracies.
      3. Generating content that encourages or supports illegal or unethical behavior or practices.
1. **Prioritize identified harms**
2. **Test and verify the prioritized harms**
   1. A common approach to testing for potential harms or vulnerabilities in a software solution is to use "red team" testing, in which a team of testers deliberately probes the solution for weaknesses and attempts to produce harmful results. 
3. **Document and share the verified harms**


**Measure potential harms**

1. Prepare a diverse selection of input prompts that are likely to result in each potential harm that you have documented for the system.
2. Submit the prompts to the system and retrieve the generated output.
3. Apply pre-defined criteria to evaluate the output and categorize it according to the level of potential harm it contains. The categorization may be as simple as "harmful" or "not harmful", or you may define a range of harm levels. Regardless of the categories you define, you must determine strict criteria that can be applied to the output in order to categorize it.

Generally a good idea to set up automated testing and periodically perform manual testing to validate new scenarios and ensure the automated testing is working as expected.

**Mitigate potential harms**
Use a layered approach

1. Model - use approriate model and fin-tune it with training data. 
2. Safety System - The safety system layer includes platform-level configurations and capabilities that help mitigate harm. For example, Azure OpenAI Service includes support for content filters that apply criteria to suppress prompts and responses based on classification of content into four severity levels (safe, low, medium, and high) for four categories of potential harm (hate, sexual, violence, and self-harm).
3. Metaprompt and grounding
   1. Specifying metaprompts or system inputs that define behavioral parameters for the model.
     1. Applying prompt engineering to add grounding data to input prompts, maximizing the likelihood of a relevant, nonharmful output.
   2. Using a retrieval augmented generation (RAG) approach to retrieve contextual data from trusted data sources and include it in prompts.
4. User experience - design the app UI to constrain inputs to specific subjects or types, apply input and output validation. Documentation about potential harms should be transparent and available to users.


**Operate a responsible generative AI solution**

* Complete Prerelease reviews
  * Legal
  * Privacy
  * Security
  * Accessibility
* Release and operate the solution
  * Devise a phased delivery plan that enables you to release the solution initially to restricted group of users. This approach enables you to gather feedback and identify problems before releasing to a wider audience.
  * Create an incident response plan that includes estimates of the time taken to respond to unanticipated incidents.
  * Create a rollback plan that defines the steps to revert the solution to a previous state in the event of an incident.
  * Implement the capability to immediately block harmful system responses when they're discovered.
  * Implement a capability to block specific users, applications, or client IP addresses in the event of system misuse.
  * Implement a way for users to provide feedback and report issues. In particular, enable users to report generated content as "inaccurate", "incomplete", "harmful", "offensive", or otherwise problematic.
  * Track telemetry data that enables you to determine user satisfaction and identify functional gaps or usability challenges. Telemetry collected should comply with privacy laws and your own organization's policies and commitments to user privacy.

