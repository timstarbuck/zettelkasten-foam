# Azure AI/Fundamentals of Facial Recognition

Use Cases

* **Security** - facial recognition can be used in building security applications, and increasingly it is used in smart phones operating systems for unlocking devices.
* **Social media** - facial recognition can be used to automatically tag known friends in photographs.
* **Intelligent monitoring** - for example, an automobile might include a system that monitors the driver's face to determine if the driver is looking at the road, looking at a mobile device, or shows signs of tiredness.
* **Advertising** - analyzing faces in an image can help direct advertisements to an appropriate demographic audience.
* **Missing persons** - using public cameras systems, facial recognition can be used to identify if a missing person is in the image frame.
* **Identity validation** - useful at ports of entry kiosks where a person holds a special entry permit.

Starts with **Face Detection** - identify regions of an image containing a human face, return a *bounding box* coordinates which form a rectangle around the face.

Microsoft Azure provides multiple Azure AI services that you can use to detect and analyze faces, including:

* **Azure AI Vision**, which offers face detection and some basic face analysis, such as returning the bounding box coordinates around an image.
* **Azure AI Video Indexer**, which you can use to detect and identify faces in a video.
* **Azure AI Face**, which offers pre-built algorithms that can detect, recognize, and analyze faces.
Of these, **Face** offers the widest range of facial analysis capabilities.
Face Service returns attributes such as:
* Rectangle coordinates of human faces.
* Accessories: indicates whether the given face has accessories. This attribute returns possible accessories including headwear, glasses, and mask, with confidence score between zero and one for each accessory.
* Blur: how blurred the face is, which can be an indication of how likely the face is to be the main focus of the image.
* Exposure: such as whether the image is underexposed or over exposed. This applies to the face in the image and not the overall image exposure.
* Glasses: whether or not the person is wearing glasses.
* Head pose: the face's orientation in a 3D space.
* Mask: indicates whether the face is wearing a mask.
* Noise: refers to visual noise in the image. If you have taken a photo with a high ISO setting for darker settings, you would notice this noise in the image. The image looks grainy or full of tiny dots that make the image less clear.
* Occlusion: determines if there might be objects blocking the face in the image.

With a Limited Access policy, customers can submit and intake form to access additional Face capabilities such as:
* The ability to compare faces for similarity.
* The ability to identify named individuals in an image.

Resources:
* **Face**: Use this specific resource type if you don't intend to use any other Azure AI services, or if you want to track utilization and costs for Face separately.
* **Azure AI services**: A general resource that includes Azure AI Face along with many other Azure AI services such as Azure AI Content Safety, Azure AI Language, and others. Use this resource type if you plan to use multiple Azure AI services and want to simplify administration and development.

Tips for better results:

* Image format - supported images are JPEG, PNG, GIF, and BMP.
* File size - 6 MB or smaller.
* Face size range - from 36 x 36 pixels up to 4096 x 4096 pixels. Smaller or larger faces will not be detected.
* Other issues - face detection can be impaired by extreme face angles, extreme lighting, and occlusion (objects blocking the face such as a hand).
