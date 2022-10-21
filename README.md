# Youtube Thumbnail Classification
## Check out the [jyputer notebook](Final_Project.ipynb)

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

### Why Deep Learning?
I think that deep learning systems, would be appropriate to explore this thumbnail classification problem because image data is quite complex. Neural networks benefit from its ability to learn from the data in increasingly abstract ways, where a key strength is its capacity to deal with non-linear relationships which may be missed by other modeling approaches. For these reasons, deep learning is more suited for solving computer vision problems compared to shallow learning approaches.

In particular, convolutional neural networks (CNNs) would be more appropriate compared to fully connected neural networks to explore my chosen problem. This is the case because fully connected neural networks suffers from a problem of inefficency since every node in every layer is fully connected to every node in the next layer. When it comes to images the resulting number of features get multiplied increasingly quickly, which substantially increases the runtime for models. On the other hand, CNNs are able to reduce the number of dimensions with the technique of running a filter through the image. This dimensionality reduction greatly improves the runtime of the models and also produces great results in regards to image data.

I believe that implementing transfer learning would also help to solve my problem at hand. I plan on using 2 transfer learning models: ResNet50 and VGG16 that have been known to work well with image data. ResNet50 was created to deal with the vanishing gradient problem with very deep neural networks and VGG16 is a very popular model for image classification problems. It will be interesting to see which model would be better suited for classifying YouTube thumbnails.

---

### Conclusion
The best model is Model 5 with a validation accuracy score of 0.71, which implements transfer learning with the VGG16 model, with an addditional fully connected layer and dropout layer added at the end.

In the following section, I will talk about the various modelling approaches I experimented with and explain why Model 5 is the best model. The first model (Model 1) I tried was a convolutional neural network (CNN) with 8 convolutional layers with a max pooling layer between every 2 convolutional layers with a dropout layer at the end. Next the model was flattened into a fully connected network with an additional fully connected layer and dropout layer, finally it was optimized using 'rmsprop' and trained with 10 epochs. Model 1 had a validation accuracy of 0.56.

Since the dataset that I have is not very big (a total of 2303 images), I decided to experiment with image augmentation to see whether introducing additional variation to my exsiting dataset would improve the model's performance. I introduced 4 different methods of augmentation, rotating (up to 45 degrees), shifting the range, horizontally flip, and zooming (up to 50%) the image. Model 2 is a copy of the modeling architechture of Model 1 with 2 additional convolutional layers and a max pooling layer, as well as using the augmented dataset rather than simply the preprocessed dataset. The resulting validation accuracy score is 0.55, which is only slightly worse than Model 1 (0.56). Since there was not a lot of improvement, I proceeded to try out transfer learning models with the augmented dataset.

Model 3 is a transfer learning network with ResNet50, I freezed the weights of the network, added a fully connected layer, a dropout layer, optimized with 'adam' at the end and trained the network with 7 epochs. The validation accuracy score is 0.42, indicating that the model is performing even worse than Model 1 (0.56) which was just a simple convolutional neural network without image augmentation. I also ran a transfer learning network with VGG16 using the augmented dataset (included at the end as an additional model) with a validation accuracy score of 0.44, which was not much different from the transfer learning network with ResNet50.

Since it appeared that image augmentation did not really improve the models I have been experimenting with. I decided to rerun the transfer learning models on the originally preprocessed dataset instead. I reran the same transfer learning with ResNet50 network on the preprocessed dataset which resulted in a validation accuracy score of 0.35. I did the same for the transfer learning with VGG16 which resulted in the highest overall validation score of 0.71.

My conclusion on using image augmentation is somewhat uncertain, it appears that image augmentation improved the transfer learning network with ResNet50 (augmented: 0.42; unagmented: 0.35). However, the validation accuracy score was much worse for transfer learning network with VGG16 (augmented: 0.44; unagmented: 0.71) and only slightly worse for my own CNN (augmented: 0.55; unagmented: 0.56). One hypothesis could be that transfer learning with ResNet50 was not the most appropriate for my dataset. Since the network was intended to tackle the problem of vanishing gradients with very deep neural networks, perhaps the YouTube thumbnails dataset did not require such a deep network and as a result did not benefit from the ResNet50 architechture. On the other hand, VGG16 was built for image classification and has been proven to perform very well on a diverse set of image classification problems. Therefore, this may be one reason why the transfer learning model with VGG16 performed so well on this problem.

I chose Model 5 (transfer learning model with VGG16) as my best model. In addition to having the best validation accuracy score (0.71), far outstriping all the other models I have experimented with, I also believe that this model is more likely to predict well on a new dataset. From the standpoint of the problem I sought to tackle it is important for the model to be able to classify the thumbnail into its content category with high accuracy. A misclassification may lead users to be confused and unable to find the videos they were searching for. In addition, if such a model was used in conjunction with another algorithim to promote similar videos, misclassification could lead users to believe that they can no longer rely on the recommendation section to find videos of interest. Overall a model with low validation accuracy may negatively impact the user experience.

Overall, when experimenting with these various models I have found that categorizing YouTube thumbnails is not a very easy problem to tackle. I have used ResNet50 and VGG16 on previous image classification problems and they have always performed very well (above 0.6 validation accuracy score). This may indicate that thumbnails are quite complex images with a lot of diversity within categories and much similarity between categories as well. However, this is just an inference based on my limited experience with transfer learning models and neural networks in general. The real answer to why these models did not perform as well may lie in the hyperparameters used which could be subject to more in-depth exploration.
