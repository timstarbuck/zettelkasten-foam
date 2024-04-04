# Azure AI/Monitor Azure AI Services

Plan for costs using pricing calculator: https://azure.microsoft.com/pricing/calculator/

Create Alerts using rules specifying:
* The scope of the alert rule - in other words, the resource you want to monitor.
* A condition on which the alert is triggered. The specific trigger for the alert is based on a signal type, which can be Activity Log (an entry in the activity log created by an action performed on the resource, such as regenerating its subscription keys) or Metric (a metric threshold such as the number of errors exceeding 10 in an hour).
* Optional actions, such as sending an email to an administrator notifying them of the alert, or running an Azure Logic App to address the issue automatically.
* Alert rule details, such as a name for the alert rule and the resource group in which it should be defined.
