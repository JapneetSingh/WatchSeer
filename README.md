# Chronos&Quartz: #### An reverse Image based search engine for watches

### Motivation and Overview
Reverse Image Search is a growing field which is largely expected to improve the way we search online
in the coming few years. Image-based features can either be used as an alternative or additional signals
to improve the efficacy of search engines. The field is growing quickly with a number of companies jumping on
the bandwagon.  I wanted to put my skills to test and develop my own version of reverse image search and chose wrist watches as test subjects.<br/>
The goal of my project was to use a picture to find similar men's wrist watches on amazon.

The results can be viewed on my app: .........................

### The Process

* __Web Scraping:__<br />
  I started out by scraping over 5000 images and associated metadata from amazon using BeautifulSoup and urllib.
  The images were stored locally while the metadata was stored in MongoDB

* __Image Featurization and PCA:__<br />
 I used OpenCV 3.0 in python for my featurization. After multiple experiments with different techniques I decided to use Edge detection, Thresholding and color histograms to create my feature space. I had around 121,000 total features by the end of the process. The features from colors(around 1500) were dwarfed by the total features from other two techniques which focus more on the shape
 and design of the watch. I captured 90 % variance for the feature space with 3625 features.

* __Metadata Featurization:__<br />
  Metadata constituted of product descriptions and product information table found on each product page. Numerical and categorical variables were handled separately. Multiple categorical variables had more than 100 unique values. They were combined to create 1 giant string and TFIDF was performed. The numerical variables were all in different units and were handles by a module Pint.  


* __Modeling and Evaluation:__<br />
  I treated this as a similarity problem and tried multiple difference units. I get my best results with *Cosine* metric for Image features and *Jaccard index* for text

  Metadata model was used to supplement the result of the Image model. Image Model is used to make the baseline predictions. We then use predictions from metadata model change the order in which images are presented as recommendations. Any watch predicted by both the models is recommended first.  
  This is because metadata model picks based on only words and does not account for patterns and shape within the .

  Since its an image based unsupervised learning problem no machine can match humans in evaluating the final results. Which is why I leave the decision in the users hands. The results seems to be doing a good job of detecting similar watches.

* __Web App__:<br\>
  A very basic version of webapp was created which uses the image url and any information you provide to recommend appropriate watches

### Code and Reproducing Results......................
You will need to start by install OpenCV to your system. The following link should help
http://stackoverflow.com/a/27650299

* __Scraping:__
The scraping code is in Image_Scraping.ipynb. Running the file once will download around 5000 images in the current working directory. This process may take about an hour to two depending upon various factors.

* __Image model:__
Code for the image model constitutes the 3 files: <br />
          * *Features.py*: Takes a single image and derives it features<br />
          * *Image_processing.py*: creates a NumPy array from all the images by calling features.py for each image<br />
          * *Model.py*: Takes the array generated by Image_processing.py and uses it to fit the model and then present the results<br />

Just run Model.py from your computer. This will take care of  creating your dataset and training your model.It will also download pickled version of a number of important objects in your working directory. The process may take some time (~20 minutes) depending on your machine.

* __Metadata Model:__

Metadata model can be created by running through the iPython notebook *Metadata_Model.ipynb*. Again this will create a pickle file for the desired models.

..............................
### Tools and Packages used

* BeautifulSoup and urllib for Webscraping
* MongoDB, OS, JSON and PyMongo for storing scraped data
* OpenCV, NumPy, Pandas, SciPy, NLTK, matplotlib and  Pint for EDA and featurization
* scikit-learn for Modeling
* AWS: S3 and EC2.................................


### Future Work:

  Currently my model works under the assumption that the the image entered by user should be in the same format as images on amazon
  i.e. with a white background and the watch placed upright.Here are some other concepts and ideas I would like to explore to add robustness to my model:<br />
    1) SIFT (Scale-invariant feature transform) and SURF (Speeded-Up Robust Features) <br />
    2) Structural Similarity (SSIM) Index<br />
    3) Neural Networks<br />


### References
1) pyimagesearch.com <br />
2) https://github.com/JapneetSingh/dimensionality-reduction <br />
3) https://github.com/nateberman/Python-WebImageScraper<br />
4) http://goo.gl/EoAAFU<br />
5) http://www.kevinjing.com/jing_pami.pdf<br/>
