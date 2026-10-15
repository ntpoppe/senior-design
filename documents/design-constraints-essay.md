Nate Poppe, Braden Hayes

CS5001

9/17/26

# Project Constraints Essay
## Blunder Risk Predictor for Chess

### Economic
Money is a real limit on this project because we're using Stockfish, which is free, instead of paying for a chess engine or a paid analysis API. That keeps costs at zero, but it also means we're stuck running everything on personal computers instead of paid cloud servers. Because of that, our model has to stay small enough to give live risk scores without needing expensive hardware. This puts pressure on how accurate the model can be, since a bigger model would likely perform better but costs more to run, and we're choosing to stay cheap over chasing top accuracy.

### Ethical
There's an ethical risk here because a live "risk score" during a game could look a lot like the engine is helping a player cheat. Sites like chess.com and Lichess both enforce a Fair Play Policy that bans engine assistance during rated games, so we have to make sure our tool is only used for reviewing games after they're over, not during live rated play. We also plan to make clear that the score is just a probability, not a guarantee, so players don't lean on it too much. This does limit the tool, since a version that worked during live games would be more useful, but we're choosing to stay on the ethical side instead.

### Professional
This project needs both real chess knowledge and applied machine learning, and getting the second one right depends on having the first. To make a risk score explainable instead of just a number, we need to understand how strong players actually think about a position, not just what the engine says. Braden already has real chess experience, which is a big part of why this project is possible at all, since figuring out what actually makes a position risky isn't something we could just look up, and it's a big part of what makes this project worth working on. This puts pressure on how we split the work, since the chess-side judgment calls rely on one teammate more than the other, and we're choosing to lean on that experience instead of trying to build the domain knowledge from scratch.
