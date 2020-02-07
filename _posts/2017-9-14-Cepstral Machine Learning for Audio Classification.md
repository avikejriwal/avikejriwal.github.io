---
layout: post
title: Cepstral Machine Learning for Audio Classification

---

### [Get the Code!](https://github.com/avikejriwal/Music-Genre-Identification)

&nbsp;

### Premise

[12 hours of content](https://www.bloomberg.com/news/features/2015-07-10/can-soundcloud-be-the-facebook-of-music-) are uploaded to SoundCloud every minute.  This presents a challenge not in terms of scaling and maintaining an increasing amount of data, but also in terms of processing and recommending new content to users.  Unfortunately, the music industry is constantly changing, and artist styles tend to vary over time, so recommendations based on track metadata are not reliable in the long term.

[The Music Genome Project](https://en.wikipedia.org/wiki/Music_Genome_Project) is an effort by music analysts to generate an exhaustive list of features to describe music at a fundamental level.  This project also forms the backbone of Pandora's recommendation engine.

With this in mind, the goal of this project is to create a model capable of classifying audio tracks by genre.  Not only would this model automate a portion of the musical feature extraction, but the components used in building the model could potentially be used to compare different songs in a recommendation system.

While music genres as a whole are inherently ambiguous, they are generally well-understood by listeners.  User preferences frequently align with specific genres, so there is value in recommending content based on genre.

### Strategy and Model

Data was collected from the [Free Music Archive](https://github.com/mdeff/fma).  In this dataset, 8000 30-second audio samples are classified into 8 distinct genres.

Audio samples were split using [Harmonic Percussive Source Separation](https://librosa.github.io/librosa_gallery/auto_examples/plot_hprss.html), then for each split component, 13 [Mel-Frequency Cepstral Coefficients (MFCCs)](https://en.wikipedia.org/wiki/Mel-frequency_cepstrum), as well as spectral contrast, were computed over several time windows.  [The mean, variance, and first and second time derivatives](https://arxiv.org/pdf/1703.09179.pdf) for each value were used as features.

An SVM with a Gaussian kernel was trained to predict genre given the statistics of the MFCC coefficients.

### Results

In classifying 8 different genres, the model achieved 54% accuracy on a test dataset of 1600 samples.

<body>
	<div id="container"></div>
  <script src="https://d3js.org/d3.v3.js"></script>
  <script src="../d3/matrix.js"></script>

  <script>
  	var confusionMatrix = [
		  [ 0.54,  0.08,  0.01,  0.08,  0.07,  0.03,  0.08,  0.02],
			[ 0.06,  0.52,  0.05,  0.06,  0.12,  0.06,  0.11,  0.06],
			[ 0.02,  0.03,  0.64,  0.02,  0.07,  0.08,  0.11,  0.04],
			[ 0.19,  0.04,  0.  ,  0.63,  0.03,  0.03,  0.08,  0.04],
			[ 0.06,  0.14,  0.09,  0.01,  0.57,  0.01,  0.08,  0.06],
			[ 0.05,  0.04,  0.09,  0.1 ,  0.02,  0.71,  0.07,  0.05],
			[ 0.08,  0.12,  0.08,  0.07,  0.06,  0.06,  0.36,  0.12],
			[ 0.01,  0.04,  0.05,  0.02,  0.05,  0.03,  0.12,  0.59]
  	];

  	var labels = ['Electronic',	'Experimental',	'Folk',	'Hip-Hop',	'Instrumental',	'International',	'Pop',	'Rock'];

  	Matrix({
  		container : '#container',
  		data      : confusionMatrix,
  		labels    : labels
  	});
  </script>
</body>

The above figure shows a [confusion matrix](https://github.com/tarobjtu/matrix) for the model on the test dataset.  The precision/recall is fairly balanced across the classes, with the exception of Pop music.  My reasoning is that Pop music is more ambiguous than other genres, as it typically overlaps with and borrows features from several different styles.

Given the inherent ambiguity and overlap in genre, one song is likely to fall under multiple genres, so it will be more effective to return multiple predictions for a single song, in order of relative probability as estimated by the model (decreasing).

With minor adjustments to the model, I was able to use the model to predict a probability distribution of genres for a given song, rather than the most likely genre.  The following plots show the distributions for 2 very popular songs.

<img src='/img/WTTJ.png'>  

<img src='/img/Despacito.png'>

In this case, Welcome to the Jungle is primarily Rock and Pop, and Despacito can be interpreted as a combination of International and Hip-Hop music.

While deep learning techniques have seen success and occasionally stronger accuracy in audio identification, this approach has the advantages of scalability and being faster to adjust and implement.

Given more time and resources, future directions with this project would include:
- Incorporating NLP and lyric data to improve genre classification
- Utilizing other audio/spectral features
- Identifying different features from audio (country, decade, etc.)

[PART 2](https://avikejriwal.github.io/Audio-Classification,-Continued/)

### Technologies

#### Python
- [pandas](http://pandas.pydata.org/)
- [librosa](https://github.com/librosa/librosa)
- [scikit-learn](https://github.com/scikit-learn/scikit-learn)  

[FFmpeg](https://www.ffmpeg.org/)

### Parting words

[Shazam's song detection algorithm is pretty cool](http://gizmodo.com/5647458/how-shazam-works-to-identify-nearly-every-song-you-throw-at-it)
