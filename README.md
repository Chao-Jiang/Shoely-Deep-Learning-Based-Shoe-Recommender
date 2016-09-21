# Shoely-Deep-Learning-Based-Shoe-Recommender
## Motivation
"A picture is worth a thousand words", you might have heard this idiom and we might not agree that one needs exactly 1000 words to describe an image but I can assure you that a picture of a shoe contains more information that I can convey with twenty something shoe-describing words that I know. That number is probably different for different groups of people and most likely I'll be in a cluster surronded by PhDs who didn't have time to look up shoes online. The fact that we cannot describe all the information in, most if not all, pictures using words only is not surprising as our languages are not as evolved as our vision and words were actualy used to communicate observations in the physical world or hallucinations.
Shoely utilizes the power of images to help shoppers find the shoes that they like at the best price. It learns about a person's style and is able to recommend new shoes based on prevoius likes/dislikes or even by looking through your photos. Shoely can although be incorporated into an image-based style recommender that can help, as an example,find the shoes that look good with other apparel. 

## Data Collection
The data for this project was collected by scraping http://www.onlineshoes.com/ for Women, Men anb Kids Shoes. For each shoe, image, category name and price were saved in a database with 40000 entries (24000 Women, 10000 Men and 6000 Kids). All the images have the same number of pixels, taken at the same angle and have white backgound.   
![screen shot 2016-09-20 at 6 03 42 pm](https://cloud.githubusercontent.com/assets/19718965/18694161/b1e4f280-7f5c-11e6-8687-20cfcb65eb4a.png)

## Image Retrieval
Image Retrieval is a classic problem in computer human interaction and is defined as: Given a query image search the database of images and return similar images. While this problem seems simple, finding a general solution to this has been a challenge for computer visionist for a long time. The problem arises from the fact that similarity measures are different for humans and computers. From a computer's point of view an image is a 3D matrix (can be expanded to a 43200(120*120*3) dimensional vector for the case of our images) and is perceived by the values of the all pixels, 

## TensorFLow for Shoes
## Sample Queries
