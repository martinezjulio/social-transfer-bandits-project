---
title: "Pre-Registration: Social Learning from Demonstrations in Novel Multi-Armed Bandits"
author: "Your Name"
date: "2024-10-22"
output: pdf_document
---

# 1. Title of the Study  
**Social Learning from Demonstrations in Novel Multi-Armed Bandits**

# 2. Hypotheses  
We predict that participants who receive demonstrations from expert teachers will show lower entropy in their multi-armed bandit choices compared to participants receiving demonstrations from novice teachers, who will exhibit higher entropy. This effect is expected in both environments: when the demonstrations take place in the same environment as the one participants play in, as well as when participants play in a novel environment. We predict a stronger entropy difference between the expert and novice demonstration conditions in the novel environment due to greater reliance on the expert demonstration and higher uncertainty in the new environment.

# 3. Study Rationale  
Humans are highly adaptive social learners, particularly when navigating unfamiliar environments. This study aims to investigate how people utilize social demonstrations from experts and novices when faced with new environments and how their performance differs with or without these social demonstrations. Understanding this can shed light on the mechanisms underlying rapid social learning in novel situations.

# 4. Methodology

## Participants  
We will recruit 400 participants aged 18-40 via Prolific. Participants will access the experiment via a URL link to the JavaScript-based game.

## Design  
This study has a mixed design. Each participant will be exposed to five randomly selected conditions out of ten possible combinations. Although not all participants experience every condition, the within-subjects design applies since participants are exposed to multiple conditions. The independent variables are:

- **demonstration_condition**: Expert, novice, or no teacher  
- **transfer_condition**: Same or new environment  
- **game_sequence_condition**: Three variations of game sequences

The dependent variables are the **entropy of arm choices** and the **total reward** in the final game of each condition.

Participants will experience five combinations of these conditions, with each condition repeated twice (10 total games). The game sequence will record participants' choices (arm pull, position, and success/failure) and compute entropy and reward for each step.

## Materials  
The task is a multi-armed bandit game implemented using HTML/JavaScript, accessible via a unique link on Prolific. The only screening question will ask if participants are colorblind, as the task requires color differentiation.

## Procedure  
Participants will receive a cover story about an "Asteroid Mining Adventure" and instructions to maximize rewards by choosing asteroids (the arms of the multi-armed bandit). They will control a starship using arrow keys to move and use spacebar/enter to pull an arm. Participants will observe other starship partners (teachers) and are encouraged to learn from their actions.

After completing a 24-trial practice game, participants will proceed through a series of games with varying environments, game sequences, and demonstration conditions. A Mining Reward Tank will display cumulative rewards, filling with green bars for successes and red bars for failures. Environment transitions occur between games when conditions change.

## Randomization  
Conditions will be uniformly and randomly assigned to each participant. The five selected condition combinations will be repeated and shuffled to determine the order.

## Blinding  
Participants will not know the underlying bandit payoff structure or the demonstration condition (expert/novice). They may infer the teacher’s expertise from observed rewards. Environment changes will not be revealed in advance.

# 5. Analyses  

## Planned Analyses  
We will use mixed-effects models to predict the **mean total reward** and **mean entropy** in the final play game for each condition.

### Regression Models for Analysis

```r
# Mixed-effects model for mean entropy
entropy_model <- lmer(
  mean_entropy ~ game_sequence_condition * transfer_condition * demonstration_condition + 
                 (1 | participant_id), data)

# Mixed-effects model for mean reward
reward_model <- lmer(
  mean_reward ~ game_sequence_condition * transfer_condition * demonstration_condition + 
                (1 | participant_id), data)
```

- game_sequence_condition: Play-watch-indicate-play vs. play-play-indicate-play
- transfer_condition: Same vs. new environment
- demonstration_condition: Expert, novice, or no teacher
- Random intercept: (1 | participant_id) accounts for repeated measures across participants.
## Key Contrasts and Interactions
- Main contrasts: Between (A) and (C), and (B) and (D). This reflects the three-way interaction:
```game_sequence_condition * transfer_condition * demonstration_condition```
- Secondary contrasts: Between (A) and (B), and (C) and (D).
## Secondary Contrast Example

```
# Contrast for demonstration condition across environments
emtrends(entropy_model, ~ demonstration_condition | transfer_condition, var = "game_sequence_condition")
```

This allows us to evaluate the difference in demonstration impact between the same and new environments.

### Data Exclusion

Participants who are colorblind or reload their game (which resets progress) will be excluded from the analysis.

### Missing Data

Participants with incomplete or missing data will be excluded. In case of data handling errors with MongoDB, affected participants will also be excluded. Missing data is expected to be rare.

# 6. Ethics and Data Collection

All data will be collected on a private server, accessible via a unique URL. MongoDB will be used to store participant data, ensuring no personally identifiable information (PII) is collected. Each participant will be assigned a unique identifier for anonymity. Data will be securely stored with restricted access, and raw data will exclude IP addresses or other PII.

# 7. Modeling

We will also develop models of human behavior using variants of Thompson sampling and Upper Confidence Bound (UCB) algorithms. Specifically:

- Neural networks (NNs) will be used to estimate prior beta parameters for the play games and another NN for the watch games.
- These estimates will be used to inform a final NN that predicts priors for the final play game.
- This modeling approach draws on prior work by Martinez, Frank, & Haber (2023), which used social demonstrations in multi-armed bandit tasks.

# 8. Key Dependent Variables

The primary dependent variables are:

1. Mean entropy: Captures uncertainty in participants’ arm choices.
2. Mean reward: Reflects total cumulative reward.
