---
layout: post
title: A Numpy Approach of the Monty Hall Problem
author: Tino Margely
---
This was my very first Data Science Project, wich is an exercise from the "OpenClassroom" e-learning platform. We first started to approach the Monty Hall Probem using only python list and functions. The objective of this exerecise was to understand the potential and efficiency of Numpy over simple Pyhon lists. 

## My first Data Science Project 
-----

### The Monty Hall Problem
> " The Monty Hall problem is a brain teaser, in the form of a probability puzzle, loosely based on the American television game show Let's Make a Deal and named after its original host, Monty Hall. The problem was originally posed (and solved) in a letter by Steve Selvin to the American Statistician in 1975 (Selvin 1975a), (Selvin 1975b). It became famous as a question from a reader's letter quoted in Marilyn vos Savant's "Ask Marilyn" column in Parade magazine in 1990 (vos Savant 1990a): 
> Suppose you're on a game show, and you're given the choice of three doors: Behind one door is a car; behind the others, goats. You pick a door, say No. 1, and the host, who knows what's behind the doors, opens another door, say No. 3, which has a goat. He then says to you, "Do you want to pick door No. 2?" Is it to your advantage to switch your choice? ", (Wikipedia)

### Model The problem with Numpy
To model this problem I first started by importing all the libraries needed and created two numpy arrays. We will give to each door a different number, from 0 to 2. The first array called choix_joueur (Player's choice) picks up a random number between 0 and 2 represents a randomly chosen door for the player. The second array called bonne_porte (Good door) represents the door hiding the car. We also introduce a variable "n" wich representing the number of games we want to simulate.
  
  ```python
import matplotlib.pyplot as plt
import numpy as np
from math import *
n=100000

choix_joueur = np.random.randint(0,3,n)
bonne_porte = np.random.randint(0,3,n)
```
 We will stock the results of the expereience in 2 numpy arrays : garder / changer (Keep /Change). 
 If you decide to keep the first door you picked, you only win when your first choice is the same as the door randomly picked (seems obvious I know). On the contrary, if you decide to change your choice, picking the good door is the only way you can lose. To explain briefly, just imagine you picked one of the two wrong doors : the host reveals a goat behind one of the two doors left. Then final door is the one behind wich the car is hiding, and as you change the door you picked first you win it !
 In terms of numpy arrays, this can be illustrated like following : 
```python
garder = bonne_porte[bonne_porte == choix_joueur]
changer = bonne_porte[choix_joueur != bonne_porte]
  ```
### Results
 
In order to get some intuituve results, I decided to give 1 point to a player everytime he wins the game and ploted the number of wins for each strategy, by playing 100,000 games first and then I compared the number of wins for each strategy depending on the number of games played.  

```python
samples =  [1000, 10000, 20000, 50000, 80000, 100000]

def play_game(n):
    choix_joueur = np.random.randint(0,3,n)
    bonne_porte = np.random.randint(0,3,n)
    garder = bonne_porte[bonne_porte == choix_joueur]
    changer = bonne_porte[choix_joueur != bonne_porte]
    
    return [garder.size, changer.size]


#Affichage des résultats

fig,(ax1,ax2) = plt.subplots(2,1,figsize=(5,8),dpi=100,num=2,sharey=True)
        
ax1.bar(['Garder','Changer'], play_game(100000), color=['r','g'])
ax1.set_title('Gains des joueurs sur 100 000 parties')

ax2.set_title("Gain des joueurs en fonction du nombre de parties")
ax2.scatter(samples, [play_game(i)[0] for i in samples],c='r')
ax2.scatter(samples, [play_game(i)[1] for i in samples], c='g')
  ```
#### And here are the results : 
![alt text](https://github.com/TinoMargely/TinoMargely.github.io/blob/master/images/result.png "Logo Title Text 1")

In the end, it is clear that the best strategy is to change the door from your first choice if you want to win the car !
