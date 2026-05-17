---
title: "Database Design via Domain Knowledge in Rollbot"
date: 2026-05-17
type: blog
tags: [devops, saas, tabletop_roleplay]
draft: false
---
## I. The Setup
I have been working on my side project, Rollbot, this week focusing on data persistence. This bot stores Discord channels, guild, and users, alongside D&D character sheets (and potentially other systems later on). These are all fairly simple to implement, but the design choices behind them could backfire later on. How do we design our database in a way that won't be problematic later on?

## II. Domain Knowledge
When we talk about the "domain", we generally mean the area of life where our product will be used. In Rollbot's case, that's online TTRPG culture, which is more varied than one might expect.

Online players have a few behaviors to account for:
### 1. Campaigns
A player has one character, played in one guild, across a long time. This is what's known as a "Campaign", where the story of the characters develops from session to session.

### 2. One-shots
A player has some characters, played in any number of guilds, each time only once. Players like these are playing "one-shots", with their favorite characters experiencing a variety of unrelated adventures.

### 3. Hybrid Usage
A player has their campaign character appear in one-shots, and later on decide whether or not the one-shots affect their campaign character. 

## III. The Solution: Relations and Variants
These cases make a few things clear: First, a character is not bound to a channel. Since the same character can be used across different games, they should not be saved as properties of channels.

Second: The same character may have different versions of itself. For example, say my campaign character Simeon is used in a one-shot where he gains a level. In his regular game, Simeon has not gained that level, but for the purposes of the one-shot it should be noted.

This led me to the idea of **character variants**: A new table where each entry represents a version of some character (via FK), with some JSON data that specifies the changes that apply to that character. In Simeon's case, that would be his new level (and only his new level). Think of variants as instances of the character.

Now that we have our character and its variants, we create a new table: channel characters. Here, we have rows with player_id, character_id, channel_id and variant_id. If player p, character c, channel g and variant v is a row, this indicates that **p plays c's variant v on channel g**:
```
Character:
    id=7
    name="Simeon"
    level=3

Variant:
    id=12
    character_id=7
    diff_data={"level": 4}

ChannelCharacter:
    player_id=5
    character_id=7
    variant_id=12
    channel_id=99
```

## IV. Evaluation
Consider again the three cases outlined before.

### 1. Campaigns
For campaign players, this system isn't necessary but also does not add extra overhead. They have one entry in the channel characters relation table, with one character variant which might as well be their basic character sheet.

### 2. One-shots
Whenever a one-shot player joins a game, they can link any of their characters, creating both a fresh variant of that character and a new entry in channel characters connecting them to that variant on the game's channel.

### 3. Hybrid Usage
For these players, it becomes easy to "fork" a campaign character into one-shots, or have one-shot characters turn into long-term campaign characters. Not only are both playstyles enabled, it's trivial to use both.

## V. Conclusion
In this case, knowing the range of player behaviors in advance, I was able to tailor my database's structure to match their needs in an elegant scalable and flexible manner. It reflects user behavior, making it easy to reason about.