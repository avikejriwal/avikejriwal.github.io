---
layout: post
title: Transfer Learning and Food

---

### [See the code on GitHub!!](https://github.com/avikejriwal/Not_Veg)

### The problem

If you've watched HBO's Silicon Valley, then you've probably heard about the infamous [Not hotdog app](https://www.youtube.com/watch?v=ACmydtFDTGs).  It was intended to be the "Shazam for food", but only ended up being able to distinguish two classes: Hotdogs and Not hotdogs.  Amusingly, it inspired the development of a real [app](https://itunes.apple.com/us/app/not-hotdog/id1212457521?mt=8) designed in Keras/Tensorflow.  The designer of the app goes into more detail [here](https://medium.com/@timanglade/how-hbos-silicon-valley-built-not-hotdog-with-mobile-tensorflow-keras-react-native-ef03260747f3).

I thought of an abstraction to the Not Hotdog model: Not vegetarian.  In particular, I was curious about designing a model to identify food images by dietary base/restriction, using fields such as dairy or vegan.  

Though it may seem like a novelty in the context of Not Hotdog, this model has potential applications in food marketing.  For instance, a restaurant could use this model to identify images to promote online if they want to cater to a specific dietary crowd.

### The data

I collected image data from Pixabay, Imgur and Google Images through their respective API's and web scrapers, and I designed the model using [VGG16](https://gist.github.com/baraldilorenzo/07d7802847aaad0a35d3) as a template in Keras.  This included images of food, as well as base products (milk jug, vegetables, raw meat, etc.).  Some example images are shown below\:

<table style="width:100%">
  <tr>
    <td><img src="/img/veg_vgg/sample_vegan.png"/></td>
    <td><img src="/img/veg_vgg/sample_dairy.png"/></td>
    <td><img src="/img/veg_vgg/sample_chicken.png"/></td>
  </tr>
  <tr>
    <td>vegan</td>
    <td>dairy</td>
    <td>meat (chicken)</td>
  </tr>
</table>

I was able to collect roughly 900 images for each class.  This number felt a bit low, so augmentation techniques such as shifting and flipping the images were used to artificially increase the size of the dataset.  VGG16 is designed to use 224x224 images as inputs, so the images had to be rescaled accordingly.  

### The model

All data collection and computation was done via a GPU instance in AWS EC2 through Keras/TensorFlow.  Training a Convolutional Neural Network (CNN) can take a while, even with GPU support, so I looked towards Transfer Learning.  With that, I would take an existing pre-trained CNN, and retrain the last few layers using my own data.  In theory it would utilize the structures and patterns learned by an existing model in predicting new labels.

The worst-case scenario for incorrect classifications, given the intended use of this model, would be promoting meals to audiences with special dietary restrictions, which risks driving away potential customers.  For instance, advertising hamburgers to vegans, or dairy to the lactose intolerant.  As such, it makes sense to prioritize precision in predicting vegan/dairy images. (i.e., make sure that the model is as confident as possible when it predicts an image to be vegan).

### The results

The labels consisted of vegan, dairy, and meat.  A confusion matrix of test results for this setup is shown below, normalized vertically.  

<img src="/img/veg_vgg/confmat_3class.png"/>

The model had a test accuracy of roughly 77%.  The model performance is fairly balanced across the three classes, though there is a bit more confusion between vegan and meat labels.

Some sample predictions are shown below.  For the most part they are pretty accurate.

<table style="width:100%">
  <tr>
    <td><img src="/img/veg_vgg/vegan_pred.png"/></td>
    <td><img src="/img/veg_vgg/dairy_pred.png"/></td>
    <td><img src="/img/veg_vgg/meat_pred.png"/></td>
  </tr>
  <tr>
    <td>vegan</td>
    <td>dairy</td>
    <td>meat</td>
  </tr>
</table>

...Though there were some occasional slip-ups:

<table style="width:100%">
  <tr>
    <td><img src="/img/veg_vgg/wrong_vegan_pred.png"/></td>
    <td><img src="/img/veg_vgg/wrong_dairy_pred.png"/></td>
    <td><img src="/img/veg_vgg/wrong_meat_pred.png"/></td>
  </tr>
  <tr>
    <td>vegan</td>
    <td>dairy</td>
    <td>meat</td>
  </tr>
</table>

The final model structure is as follows:

<img src="/img/veg_vgg/model.png"/>

### Parting thoughts and Future directions

It's evident that an image alone may not be sufficient to identify vegan/dairy/meat in some cases.  For instance, it is possible that the cake could be vegan, or that a vegetable soup was made with chicken broth.

Vegetarian/nonvegetarian aside, there are other kinds of diets that can be targeted, such as paleo or keto.  One idea for a future direction would be to build an image classifier to identify meals supported by a specific diet.  Another idea could be to train an image classifier to identify doneness in cooking meat.
