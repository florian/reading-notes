# Game Theory: A Nontechnical Introduction

## 1. An Overview

- Game theory is the science of decision making
- *Strategies* describe how *players* act (*Note: RL policy*)
- The *payoff* is the reward a player gets at the end of the game (*Note: RL reward*)
- We assume players are rational and ignore psychological aspects
- In *games with perfect information*, all players have full knowledge of the state of the game

## 2. The Two-Person, Zero-Sum Game with Equilibrium Points

- *Zero-sum* games: Players have opposite interest. Player A's gain is player B's loss. Elections are zero-sum games, trade is generally not
- Two strategies are in equilibrium if neither player improves by changing their strategy (*Note: Seems like a local optimum*)
- In two-person, zero-sum games all equilibrium points have the same payoff. In non-zero-sum games this does not have to be the case, as an alternative outcome might be preferrable to both players
- If we assume B knows A's strategy, then B will try to minimize the row that A chooses. A should then choose the maximum of these minimum values. This is called the *maximin*. The opposite is called the *minimax*. If both values are the same, an equilibrium has been found
- If an equilibrium exists, we call it the *solution* to the game and its payoff the *value* of the game
- The value is what a player playing according to their equilibrium strategy can expect as a minimum payoff and what they can expect as a maximum payoff for their opponent
- Zero-sum means it is in a player's interest to minimize the payoff of their opponent
- Payoffs of non-equilibrium points have no effect on the outcome of the game
- A strategy is dominating another if its payoffs are always at least as good and at least once better
- In zero-sum games, you can assume that you or your opponent never use a dominated strategy. This can help to figure out an equilibrium

## 3. The General, Two-Person, Zero-Sum Game

- Not all games have equilibrium points as discussed in the previous chapter
- In those cases, your opponent could try to second guess your strategy. If you use a pure strategy, they could ensure that you always lose. For example, when the game is trying to predict what side of a coin the opponent chooses, they could always choose the one we would not have guessed
- The solution to this dilemma are mixed strategies, i.e. making choices with different probabilities. In the case of the coin game, we would choose each side with equal probability. Now, it again does not matter whether our opponent tries to guess our choice
- If our opponent does not know about this, we could try to play a different strategies, but this fails as soon as we play against a more knowledgable opponent
- > The more capable your opponent, the more attractive the randomization process becomes
- One of the fundamental theorems of game theory is Von Neumann's minimax theorem: We can assign a value to every finite, two-person, zero-sum game that tells us how much player I can expect to win on average
- There is a mixed strategy that protects this return for player I and a mixed strategy for player II that stops them from losing any more. Since the game is zero-sum, both players use those strategies if they act rationally
- > The virtue of the minimax strategy is security
- All of this works because we assume we are playing against a perfect opponent
- To find such a mixed strategy we need to solve an equation system with some constraints. The equation system is based on the fact that the payoff should stay the same regardless of what the opponent does
- In experimental studies few people actually use equilibrium strategies against experienced opponents. However, when the opponent plays randomly, they do choose the strategy with the highest average return
- Sometimes the assumption that we want to maximize the average return is flawed since it assumes we are playing for a long time. If we can only play a few times, it might make more sense to go for a smaller but safer payoff. This is the reason why many legal cases are settled in court
