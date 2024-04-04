# Azure AI/Fundamentals of Optical Character Recognition

Optical character recognition (OCR) enables artificial intelligence (AI) systems to read text in images, enabling applications to extract information from photographs, scanned documents, and other sources of digitized text.

Azure AI Vision service has the ability to extract machine-readable text from images. Azure AI Vision's *Read API* (aka *Read OCR Engine*) is the OCR engine that powers text extraction from images, PDFs, and TIFF files. OCR for images is optimized for general, non-document images that makes it easier to embed OCR in your user experience scenarios.

Calling the Read API returns results arranged into the following hierarchy:
* **Pages** - One for each page of text, including information about the page size and orientation.
* **Lines** - The lines of text on a page.
* **Words** - The words in a line of text, including the bounding box coordinates and text itself.

Resources:
* **Azure AI Vision**: A specific resource for vision services. Use this resource type if you don't intend to use any other AI services, or if you want to track utilization and costs for your AI Vision resource separately.
* **Azure AI services**: A general resource that includes Azure AI Vision along with many other Azure AI services such as Azure AI Language, Azure AI Speech, and others. Use this resource type if you plan to use multiple Azure AI services and want to simplify administration and development.

Once you've created a resource, there are several ways to use Azure AI Vision's Read API:
* Vision Studio
* REST API
* Software Development Kits (SDKs): Python, C#, JavaScript
