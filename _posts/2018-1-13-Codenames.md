---
layout: post
title: A Codenames Bot Design Challenge using word2vec

---

### [See the code on GitHub](https://github.com/avikejriwal/Codenames)

Previously I mentioned playing Settlers of Catan over the holidays.  I've still been playing a lot of that, but I also introduced my family to a new board game: [Codenames](https://boardgamegeek.com/boardgame/178900/codenames).  I was curious to see if I could find some way to apply my background to the game (in particular, I wanted design a bot to play the role of Spymaster).  In this post I'll explore how I went about that.

### A brief explanation of the game.

Codenames is a word association game, in which two teams are trying to identify and make contact with their agents.  The agents are secret words in from a list of nouns, arranged in a 5 x 5 grid.  Two players, one for each team, play the role of Spymaster, in which they take turns giving one-word clues to their teammates to help identify their respective agents.  The winning team is the first to make contact with all of their agents without contacting the assassin (guessing the assassin = automatic loss).

The challenge is not only in giving accurate one-word clues, but in giving clues that can point to multiple agents at one time (it is a race, after all).  Another challenge is in giving clues that are unique enough that they don't encourage teammates to guess codenames that are neutral (civilians), belonging to the other team, or worst of all, the assassin.

### Model

This game is all about finding similarity between words, so it seemed intuitive that a word embedding would be useful.  For that, I used [Google's pretrained word2vec model](https://code.google.com/archive/p/word2vec/), which formed the foundation of the bot.  

With the current model, the bot uses the word embedding to generate a number of neighboring/similar words to each of the target codenames, then uses the most frequently occurring word as a clue.

### Simulation and testing

I implemented a basic simulation of the game, with the player and a bot Spymaster competing against the computer.  The game is played solely through the command line (PowerShell).

The model performs reasonably well, being able to give fairly effective (though sometimes a little cryptic) clues.  I'm fairly happy with the initial results, though there is room for improvement.

Some ideas for improvement:
- introduce a system for penalizing words similar to the civilians, the assassin, or the opponent's codenames when generating clues
- use of the zero rule (the Spymaster can give a clue with the number 0, indicating that no codename relates to that word, as well as allowing for unlimited guesses that turn)  
