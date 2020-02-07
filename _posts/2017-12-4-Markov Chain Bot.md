---
layout: post
title: Markov Chains and Mad Scientists

---

### [See the GitHub repo here](https://github.com/avikejriwal/MortyBot)

I'm a huge fan of the sci-fi cartoon *Rick and Morty*.  I was interested in replicating some of the personalities in the show, so I went out to design a Twitter bot to do exactly that.

### Collecting the data

For sample text, I used episode transcripts collected from [Rickipedia](http://rickandmorty.wikia.com/wiki/Rickipedia).  I used BeautifulSoup in Python to parse through the website and scrape the transcript pages.
With these, I compiled a list of quotes for individual characters.  For this project, I chose to base the bot around Morty, one of the title characters.

### The model

For the quote generation model, I used a [Markov chain](https://en.wikipedia.org/wiki/Markov_chain).  Simply put, a Markov chain is a random process with states changing over time, in which each state is only dependent on the state immediately before it.  

For text, each individual word in a sentence or paragraph can be thought of as a state in a Markov chain.  So starting with a seed word, the next words are chosen randomly based on their frequency of presence in the corpus of text (the list of quotes).  The model was built in Python using [markovify](https://github.com/jsvine/markovify).

### Deploying on Twitter

After collecting the data and building a text generation model, the next step is to broadcast Morty's thoughts to the world.  I deployed my bot on an AWS EC2 instance, where it sends tweets through the Twitter API and [tweepy](http://www.tweepy.org/).

You can see the results [here](https://twitter.com/The_Mortiest).  The tweets are generated using a somewhat random process, so not all of them are guaranteed to make sense.  Nonetheless, I hope that they can still generate a few laughs.

PS: [Morty does frequently say "Oh Geez."](https://www.youtube.com/watch?v=NivwAQ8sUYQ)
