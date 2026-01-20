---
name: x-algorithm-posting
description: Expert guidance for writing X (Twitter) posts optimized for the algorithm. Use this skill when writing tweets, creating tweet threads, planning content strategy for X, or when the user wants to maximize reach and engagement on X/Twitter. Provides algorithm-backed strategies for content creation, optimal posting patterns, and engagement tactics based on analysis of the open-source X recommendation algorithm.
---

# X Algorithm Posting Strategy

This skill provides algorithm-backed guidance for creating high-performing X (Twitter) content.

## Quick Reference: What the Algorithm Rewards

### Positive Signals (BOOST your score)
| Signal | Weight | How to Trigger |
|--------|--------|----------------|
| Replies | HIGH | Ask questions, share opinions, create discussion |
| Reposts | HIGH | Make content quotable and shareable |
| Quote Tweets | HIGH | Create content others want to add their take to |
| Shares (DM) | MEDIUM | Useful tips, humor, "send this to a friend" content |
| Shares (Copy Link) | MEDIUM | Bookmark-worthy, reference content |
| Dwell Time | MEDIUM | Compelling hooks, depth that rewards attention |
| Profile Clicks | MEDIUM | Mysterious/interesting author persona |
| Video Quality Views | MEDIUM | Videos watched past minimum duration |
| Likes | LOW | Basic approval signal |
| Follows | HIGH | Strongest author-level signal |

### Negative Signals (TANK your score)
| Signal | Impact | What Triggers It |
|--------|--------|------------------|
| "Not Interested" | NEGATIVE | Irrelevant or annoying content |
| Block | VERY NEGATIVE | Spam, harassment, offensive content |
| Mute | NEGATIVE | Posting too much, annoying patterns |
| Report | VERY NEGATIVE | Policy violations, harmful content |

**Critical:** One block can undo 10+ likes. Avoid triggering negative signals at all costs.

## Scoring Formula

```
Final Score = Σ (weight × P(action)) for each action
```

The algorithm predicts probability of each action and applies weights. Positive actions add to score, negative actions subtract.

## Content Creation Guidelines

### The Optimal Post Structure

1. **Hook** (First line)
   - Must stop the scroll
   - Create curiosity or tension
   - Front-load value proposition

2. **Body**
   - Deliver on the hook's promise
   - Include depth that rewards dwell time
   - Make it quotable

3. **Engagement Driver**
   - Ask a question
   - Invite opinions
   - Create reason to reply

### What to Include

- **Questions** - Drive replies (highest weighted signal)
- **Controversial takes** - Drive quote tweets and discussion
- **Useful information** - Drives DM shares and saves
- **Media** - Photos get expanded, videos get VQV bonus
- **Threads** - Avoid spam penalty vs multiple separate posts

### What to Avoid

- **Spam/high frequency posting** - Author diversity penalty
- **Engagement bait** - Triggers "not interested"
- **Rage bait** - Short term gain, long term algo punishment
- **Empty posts** - Low dwell time hurts score
- **Overt promotion** - Triggers blocks/mutes

## Key Algorithm Mechanics

### 48-Hour Window
Posts have ~48 hour shelf life in the system. Front-load engagement in first 2-4 hours.

### Author Diversity Penalty
Multiple posts from same author get progressively lower scores:
```
multiplier = (1 - floor) × decay^position + floor
```
Recommendation: 2-4 posts per day maximum.

### In-Network Priority
Posts from followed accounts get priority over out-of-network discovery. Growing followers = more reach.

### Video Bonus
Videos above minimum duration threshold get VQV (Video Quality View) scoring bonus.

## Strategic Tactics

### Reply Strategy
Reply early to viral posts from large accounts. Your reply becomes part of their thread, exposed to their audience.

### Quote Tweet > Retweet
Quote tweets give you exposure PLUS your take appears in feeds. Regular retweets only boost the original.

### Thread Strategy
Threads count as ONE post with multiple parts, avoiding the spam penalty. Long threads > multiple single tweets.

### Profile Click Bait
Make people curious about you:
- Incomplete/mysterious bio hooks
- Hot takes that make people wonder "who is this?"
- Unpopular opinions

### DM Shareability
Create content people want to share privately:
- Useful tips/tutorials
- Relatable memes
- "You need to see this" moments

## Content Templates

### Question Post
```
[Controversial statement or observation]

[1-2 sentences of context]

What do you think?
```

### Thread Hook
```
[Big promise or revelation]

Here's what I learned:

🧵
```

### Quotable Take
```
[Strong opinion stated simply]

[Brief supporting argument]

[Optional: "Change my mind" or similar engagement driver]
```

### Useful Content
```
[Problem your audience has]

Here's how to solve it:

[Step-by-step solution]

[Save this for later]
```

## For Detailed Reference

See [references/algorithm-deep-dive.md](references/algorithm-deep-dive.md) for:
- Complete list of all 15+ tracked engagement signals
- Technical details of the Phoenix ranking model
- Filter rules that remove posts from feeds
- Two-tower retrieval system explanation
