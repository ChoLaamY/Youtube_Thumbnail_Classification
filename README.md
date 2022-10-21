# Youtube Thumbnail Classification

### Intro
I was interested in exploring an image classification problem tied to something that I interacted with or used on a daily basis. One particular problem I am interested in is how tags are generated when videos or images are uploaded onto a digital platform. For example, I found that I could filter for different images of objects or people on the iPhone. Such that if I typed 'cat' into the search bar, I would get a selection of all the identified images of my cat from my photo album. This problem is related to object detection, but I believe that it is more broadly related to the similar problem of image classification. I perceive object detection to be a more straightforward problem, where there is more or less no overlap between categories. As such, I am more interested in whether neural networks can identify different themes from images that have objects that could belong to more than 1 category.

---

### Overview
For this project I am working to identify the content category (e.g. News, Food, Science, etc.) based on YouTube thumbnails. I believe this that the classification of thumbnails is relevant to the user experience of watching videos on a platform such as YouTube. Thumbnails are almost always representative of the content of the video, for example a thumbnail of a person holding a huge burger most likely belongs to a Food channel. As such assigning accurate tags to video thumbnails can help users filter for videos of interest based on such tags and aid the algorithm in recommending other videos based on similar tags.

This is an interesting problem to me because there is both a diversity of thumbnails within categories as well as similarity of thumbnails between categories. More concretely, a thumbnail from a News channel could be of a news anchor seated professionally at a table or an image of an explosion from some newsworthy incident. From a object detection point of view the 2 images are completely unrelated as one depicts a person and the other depicts a scene. However, from a classification point of view, a neural network may be able to identify some similarity in the image beyond the objects involved. An example of similarity is an image of a person laughing which could be from a Comedy channel but could also belong to a Gaming channel. As an object detection problem these images would be classified as the same thing (i.e. a person). However, from a classification perspective perhaps there is something about the colors or positioning that could diffrentiate the 2 images. For these reasons, I believe that there is a lot of complexity in classifying these images into their respecctive categories and I am interested to explore how well neural networks will work for this specific problem.

### Data
I am working with image data downloaded from [Kaggle](https://www.kaggle.com/datasets/praneshmukhopadhyay/youtube-thumbnail-dataset) that has been classified into 10 categories:
* Automobile
* Blog
* Comedy
* Entertainment
* Food
* Informative
* News
* Science
* Tech
* Video Games

However, since the dataset is quite small with only a total of 2303 images and the number of images within each category is quite uneven (min: 160; max: 300). I have decided to combine some categories together to create 3 large categories instead to better train my model for this problem. I elected to combine categories that are similar in content to one another and more distinct between categories, resulting in 3 large categories.
* **Entertainment** (813 images)
    * Blog
    * Comedy
    * Food
    * Entertainment
* **Informative** (716 images)
    * News
    * Science
    * Informative
* **Tech** (774 images)
    * Automobile
    * Video Games
    * Tech
