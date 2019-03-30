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
