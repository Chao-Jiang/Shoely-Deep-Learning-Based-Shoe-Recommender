# Shoely-Deep-Learning-Based-Shoe-Recommender
## Motivation
"A picture is worth a thousand words", you might have heard this idiom and we might not agree that one needs exactly 1000 words to describe an image but I can assure you that a picture of a shoe contains more information that I can convey with twenty something shoe-describing words that I know. That number is probably different for different groups of people and most likely I'll be in a cluster surronded by PhDs who didn't have time to look up shoes online. The fact that we cannot describe all the information in, most if not all, pictures using words only is not surprising as our languages are not as evolved as our vision and words were actualy used to communicate observations in the physical world.
Shoely utilizes the power of images to help shoppers find the shoes that they like at the best price. It learns about a person's style and is able to recommend new shoes based on prevoius likes/dislikes or even by looking through your photos. Shoely can although be incorporated into an image-based style recommender that can help, as an example,find the shoes that look good with other apparel. 
## Data Collection
The data for this project was collected by scraping http://www.onlineshoes.com/ for Women, Men anb Kids' Shoes. For each shoe, image, category name and price were saved in a database with 40000 entries (24000 Women, 10000 Men and 6000 Kids). All the images have the same number of pixels, taken at the same angle and have white backgound. 
<p align="center">
  <img src="https://cloud.githubusercontent.com/assets/19718965/18694161/b1e4f280-7f5c-11e6-8687-20cfcb65eb4a.png">
</p>
## Image Retrieval
Image retrieval is a classic problem and is defined as: Given a query image search the database of images and return similar images. While this problem seems simple, finding a general solution to this has been a challenge for computer visionist for a long time. The problem arises from the fact that similarity measures are different for humans and computers. From a computer's point of view an image is a 3D matrix (can be expanded to a 43200=120x120x3 dimensional vector in the case of our images) which is defined by all these pixel values. We, on the other hand, percieve images as a collection of objects in a much lower dimensional space. The problem then becomes transferring the images from pixel space to a feature space where the features are the same (or equivalent) to what a human brain sees in an image. Differnt solutions are proposed for dimensioanlity reduction in images are proposed (Principal Component Analysis, Image Hashing, Bag of Words and Deep Learning Convolutional Neural Networks). 
Before discussing the transformations to lower dimensional feature space, let's look at similarity in the pixel space. The simialarity of two vectors (agian our images are 43200 dimensional vectors) is calculated using Peasrson or Cosine similarity formulas. If the shoes have the same orientation and are located in the same section of the image, the similar shoes will have higher Pearson coefficient. This is the case for the images in our database and as you can see this simple approach works fairly well.
<p align="center">
  <img src="https://cloud.githubusercontent.com/assets/19718965/18696515/97a03f9e-7f6e-11e6-9dbb-e5e1f17f20e4.png">
</p>
Calculating Pearson coefficient of the query image against all the images in database (in the original pixel domain) is not efficient and increases the run time. Note that all the backend calculations should be completed in a few seconds. One apprach will be to divide the databse into different clusters and compare the query image with a representative image of each cluster and perform the caculation only in the most similar cluster. This can significantly reduce the computational time, however, it turns out that most of the clustering algorithms (k-Means, ...) fail at this high dimensions (CURSE OF DIMENSIONALITY) and we can only get approximate nearest neighbors. We used Approximate Nearest Neighbors Oh Yeah! [ANNOY](https://github.com/spotify/annoy) and as you'd expect it works well for images that are well algined with those of the database. You'll see its performance in the "Sample Queries" section.
Before heading to the next section, below is a cool picture of EigenShoes that is generated using 30 randomly genearted Men's shoes.  
<p align="center">
  <img src="https://cloud.githubusercontent.com/assets/19718965/18697195/d5516a8e-7f73-11e6-8934-0f92f10c1253.png">
</p>
## Feature Selection using Deep Neural Networks
We have added a layer of shoe images to AlexNet. [AlexNet](https://github.com/BVLC/caffe/tree/master/models/bvlc_alexnet) is a pre-trained convolutional neural network that has been trained on ~1,000,000 images. After getting trained on the shoe images, by adding only one layer to AlexNet, the new deep CNN can now transform the input shoe image into a 40 dimensional feature space. It is then much easiers to find similar images in the new space (using kNN for example). This method really shines, as you'll see in the next section.  

## Sample Queries
In this section, we show few queries that are designed to compare three different algorithms that are described above namely BruteForce Similarity Search, Approximate Nearest Neighbors and Deep Learning. 
In this case all three methods return acceptable results.
<p align="center">
  <img src="https://cloud.githubusercontent.com/assets/19718965/18697612/c0a57310-7f77-11e6-8091-5b4b3c87d11b.png">
</p>
BruteForce returns results that are not are very satisfing. This is cause by pixel level and possibly colors. 
<p align="center">
  <img src="https://cloud.githubusercontent.com/assets/19718965/18697621/d1c98960-7f77-11e6-917e-7c5375eead02.png">
</p>
This image is different from iamges in the database and is stretched along the diagonal. While BruteForce and ANNOY completely fail, deep learning returns leather shoes and at least two of them can be acceptable. Note that this is limited database and we might not have more similar images. 
<p align="center">
  <img src="https://cloud.githubusercontent.com/assets/19718965/18697627/e8e7ba18-7f77-11e6-87d0-d96f70fdf488.png">
</p>
This is where deep learning truely shines. The other two algorithms completely fail which is mainly driven by the fact that query image has a different angle than the database images.
<p align="center">
  <img src="https://cloud.githubusercontent.com/assets/19718965/18697628/eb7f63e8-7f77-11e6-8b73-7e1f27207df0.png">
</p>
