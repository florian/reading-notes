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
- In two-person, zero-sum games all equilibrium points have the same payoff. In non-zero-sum games this does not have to be the case, as an alternative outcome might be preferable to both players
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
- If our opponent does not know about this, we could try to play a different strategies, but this fails as soon as we play against a more knowledgeable opponent
- > The more capable your opponent, the more attractive the randomization process becomes
- One of the fundamental theorems of game theory is Von Neumann's minimax theorem: We can assign a value to every finite, two-person, zero-sum game that tells us how much player I can expect to win on average
- There is a mixed strategy that protects this return for player I and a mixed strategy for player II that stops them from losing any more. Since the game is zero-sum, both players use those strategies if they act rationally
- > The virtue of the minimax strategy is security
- All of this works because we assume we are playing against a perfect opponent
- To find such a mixed strategy we need to solve an equation system with some constraints. The equation system is based on the fact that the payoff should stay the same regardless of what the opponent does
- In experimental studies few people actually use equilibrium strategies against experienced opponents. However, when the opponent plays randomly, they do choose the strategy with the highest average return
- Sometimes the assumption that we want to maximize the average return is flawed since it assumes we are playing for a long time. If we can only play a few times, it might make more sense to go for a smaller but safer payoff. This is the reason why many legal cases are settled in court

## 4. Utility Theory

- If we can afford to play long enough and can shoulder the risk, maximizing the average return is the perfect thing to do
- In many situations people prefer to keep risk at a low amount instead of maximizing the average return
- Some economists argue that this is one of the reasons why the rich become richer. They can afford to invest in high-risk high-return ventures that the middle class would avoid
- People even play games with a negative average return by signing up for insurances
- A *utility* function maps potential rewards to numerical payoffs
- As a consequence it can also be used to assign value to distributions over rewards
- Utility functions should follow certain conditions, e.g. they should be consistent and transitive
- Experimentally it has been shown that people don't always follow utility functions with these conditions
- However, things are not as bad if we control for external factors, e.g. people act in a less risky way when alone
- People generally decrease the risk they want to take after they already won something. People also act differently depending on how a question is framed

## 5. The Two-Person, Non-Zero-Sum Game

- Zero-sum games are easier to analyze, but are also rather uncommon
- We can think of games as falling somewhere on a scale between competitive and cooperative. Zero-sum games are completely competitive. Completely cooperative games are not that interesting since players just need to communicate well
- In all other games, players will do worse if they both are purely trying to optimize their own gain
- In non-zero-sum games the payoff matrix does not tell the entire story. It makes a difference whether players can communicate, only communicate in the beginning, split payoffs, and whether agreements are binding
- The more cooperation a game requires, the more communication helps
- Players can indirectly communicate by choosing certain strategies if a game is played multiple times. This way they can show the other player that they want to choose a cooperative strategy
- Actually the perfect response to this is to ignore the other player and cash out for a while, just until before the other player is about to give up. Eventually both players need to cooperate, but before that, one of the players can get additional payoffs
- The player that makes the first move can have an advantage because they can force the other player into a bad situation
- Keeping information secret and giving away certain information can both be advantages
- Threats can be modelled using game theory
- Players are much more likely to cooperate in the prisoner's dilemma if the game is repeated indefinitely
- Two players bargaining are in an awkward position, they must cooperate but not too much. The player that takes the first step to an agreement usually has a disadvantage. Rich players have a huge advantage in this game since they generally care less whether an agreement is made at all, thus they can get a better deal
- In experiments, players rarely cooperate even though it would be to their advantage to do so a little bit. The reason might be that they are viewing the game as a competition
- Evolution and how mutations spread can be modelled using game theory
- An evolutionary stable strategy is a mutation that is never completely erased from a population
- An interesting variation to an auction is making the second highest bidder pay as well. While this is not realistic for actual auctions, it can be used to model a lot of other situations (e.g. time investment)
- Some character traits correlate with how much players are willing to cooperate

## 6. The n-Person Game

- Elections can be modelled as n-person games
- The power of a player shows how much their strategy (vote) can influence a game
- We would like to design elections in a way that each person's vote has the same power. This is difficult, e.g. in small states. While people in large states have less power inside the state, the combined power of the state often more than makes up for that
- Coalitions can be formed in n-player games. Players expect a certain payoff for entering a coalition
- Aumann-Maschler theory can be used to compute the payoff for each player if they decide to enter a coalition. It however does not say who will enter a coalition. Sometimes there's no certain choice and often it also depends on psychology
- If you're on the extreme side of a political spectrum, it can make sense to not vote at all to show that your favored party should move in that direction
- *Superadditivity*: Given two disjoint coalitions, their union will at least have the value that the individual ones have
- Game theory cannot model all variables, e.g. bargaining skills or norms of society
- Neumann-Morgenstein (N-M) theory aims to find solutions for n-player games
- N-M first eliminates all solutions that are not Pareto-optimal, i.e. no solutions are kept where all players can simultaneously do better with a different solution
- Solutions in n-player games: sets of coalitions that do not dominate each other
- Effective set of a coalition is a set of players that have enough power to convince the group to distribute payoffs differently
- Not all n-player games have solutions
- N-M assumes there's perfect communication between players
- Aumann-Maschler (A-M) is a second theory that approaches the problem in a different way
- Given a possible coalition, a player could *object* by proposing a coalition in that all players except another player do better. The other player then has to come up with a *counterobjection* to avoid being left out
- N-M and A-M have a different concept of solutions and make different assumptions
- The *Shapley value* tries to describe the value of the game for each player. It is derived by iteratively forming larger coalitions, looking at the possible ways this can be done, and what payoffs this will yield
- Power in a vote: the portion of time the vote of the respective player makes the difference, i.e. a power of 0 means the player has nothing to say, 1 means they decide everything. The power is computed by looking at all permutations of vote order and checking whose vote brings the number of votes above 50%
- This power index has been used to sue against power distributions in certain states since it can be used to show that some cities have nearly no influence
- Logrolling, agreeing to vote for something in return for help at another vote, can actually hurt everybody involved
- Arrow's theorem: There is no decision rule that always perfectly translates the desires of society into votes
