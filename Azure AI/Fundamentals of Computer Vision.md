# Azure AI/Fundamentals of Computer Vision

Images are represented by two-dimensional array of pixels. Colors are channels.

A common way to perform image processing tasks is to apply filters that modify the pixel values of the image to create a visual effect. 
For example a *convolutional filter* can be applied to find the edges of objects.

Convolutional neural networks (CNNs) 
: use filters to extract numeric feature maps, then feeds these into a deep learning model to generate a label prediction. Image classification - label represents the main subject of the image.

Multi-Modal models
: model is trained with no fixed labels, Image encoder extracts features from images based on pixel values and combines them with text embeddings created by a language encoder. 

The Microsoft *Florence* model is a Multi-modal model training with huge volumes of captioned images from internet. It is an example of a *foundation* model, a pre-trained general model on which you can build multiple adaptive models for specialist tasks:
* Image classification: Identifying to which category an image belongs.
* Object detection: Locating individual objects within an image.
* Captioning: Generating appropriate descriptions of images.
* Tagging: Compiling a list of relevant text tags for an image.

**Azure AI Vision**
Resources: 
* **Azure AI Vision**: A specific resource for the Azure AI Vision service. Use this resource type if you don't intend to use any other Azure AI services, or if you want to track utilization and costs for your Azure AI Vision resource separately.
* **Azure AI services**: A general resource that includes Azure AI Vision along with many other Azure AI services; such as Azure AI Language, Azure AI Custom Vision, Azure AI Translator, and others. Use this resource type if you plan to use multiple AI services and want to simplify administration and development.

Azure AI Vision supports multiple image analysis capabilities, including:
* Optical character recognition (OCR) - extracting text from images.
* Generating captions and descriptions of images.
* Detection of thousands of common objects in images.
* Tagging visual features in images

Azure AI Vision Studio can be used.
