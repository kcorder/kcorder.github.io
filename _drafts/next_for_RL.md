---
layout: post
title: "My first draft post"
---

# What's Next for RL? 

RL has seen many significant advances in the past few years, mostly as a result of deep learning advances. However, many large hurdles remain. In this post, I give my shortlist of (the long list of) *large* advances that I hope to see in research. 

1. Efficiently re-using memory 
  - cite MERLIN, neural Turing machines, and other RL with LSTM works. 
2. Significant model-based learning (approximate modeling) 
  - People are excellent at transfer learning, likely because we have experience and can expect our actions to have certain outcomes. 
  - Advances in quickly learning the model of the world, and how it relates to past experience of similar worlds, is essential for fast learning. 
3. Transfer learning/multi-task learning
  - tasks that are similar should obviously inform better actions for each other
    - e.g., playing Atari games 
4. Multi-agent learning 
  - important to recognize other agents in the environment and consider their beliefs, goals, and probable actions. Assuming all others are part of the stochastic environment will perform poorly. 
  - A problem with this is scale: modeling all other agents would be very costly. How is this handled normally? 
5. Hierarchical learning 
  - Learning at the lowest level will necessarily have a huge state-action space. This is not how humans plan. We make high level plans (e.g., go to the store) and fill in the details with smaller plans (e.g., leave the house, then get in car, etc.). Learning joint motor controls on a robot is analogous to plan going to the store with individual muscle contractions---very difficult to imagine this complexity. 
