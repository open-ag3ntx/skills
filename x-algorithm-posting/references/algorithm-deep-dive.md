# X Algorithm Deep Dive

Complete technical reference for the X (Twitter) recommendation algorithm.

## Table of Contents
- [System Architecture](#system-architecture)
- [Content Sources](#content-sources)
- [Phoenix Scoring Model](#phoenix-scoring-model)
- [All Tracked Engagement Signals](#all-tracked-engagement-signals)
- [Score Modifiers](#score-modifiers)
- [Filters That Remove Posts](#filters-that-remove-posts)
- [User Timeline Optimization](#user-timeline-optimization)



## Phoenix Scoring Model

Uses a **Grok-based transformer** that predicts engagement probabilities.

### How It Works
1. Takes user context (engagement history) + candidate posts as input
2. Uses special attention masking so candidates cannot see each other
3. Outputs probability for each action type
4. Weighted combination produces final score

### Candidate Isolation
Each candidate post is scored independently. The score for post A doesn't depend on which other posts are in the batch. This makes scores consistent and cacheable.

---

## All Tracked Engagement Signals

### Positive Signals (Add to Score)

| Signal | Code Name | Description |
|--------|-----------|-------------|
| Like | `favorite_score` | User likes the post |
| Reply | `reply_score` | User replies to the post |
| Retweet | `retweet_score` | User retweets the post |
| Quote Tweet | `quote_score` | User quote tweets the post |
| Click | `click_score` | User clicks on the post |
| Profile Click | `profile_click_score` | User clicks author's profile |
| Photo Expand | `photo_expand_score` | User expands photo |
| Video Quality View | `vqv_score` | User watches video past threshold |
| Share | `share_score` | User clicks share button |
| Share via DM | `share_via_dm_score` | User shares via direct message |
| Share via Copy Link | `share_via_copy_link_score` | User copies link |
| Dwell | `dwell_score` | User dwells on post |
| Dwell Time | `dwell_time` | Actual time spent (continuous) |
| Quoted Click | `quoted_click_score` | User clicks on quoted content |
| Follow Author | `follow_author_score` | User follows the author |

### Negative Signals (Subtract from Score)

| Signal | Code Name | Description |
|--------|-----------|-------------|
| Not Interested | `not_interested_score` | User clicks "Not Interested" |
| Block Author | `block_author_score` | User blocks the author |
| Mute Author | `mute_author_score` | User mutes the author |
| Report | `report_score` | User reports the post |

---

## Score Modifiers

### Weighted Scorer
Combines all predictions into final score:
```
Score = Σ (weight_i × P(action_i))
```

### Author Diversity Scorer
Attenuates repeated author scores:
```
multiplier = (1 - floor) × decay^position + floor
```
- Position 0 (first post): Full score
- Position 1: Reduced score
- Position 2+: Further reduced

Purpose: Ensure feed diversity, prevent single-author flooding.

### Out-of-Network (OON) Scorer
In-network content prioritized:
```
if out_of_network:
    score = score × OON_WEIGHT_FACTOR  // Factor < 1.0
```

### Video Quality View (VQV) Eligibility
Videos get bonus only if duration exceeds minimum:
```
if video_duration_ms > MIN_VIDEO_DURATION_MS:
    apply VQV_WEIGHT bonus
```

### Score Normalization
Final scores are normalized and offset to handle negative combined scores.

---

## Filters That Remove Posts

### Pre-Scoring Filters (Candidates Eliminated Before Scoring)

| Filter | What Gets Removed |
|--------|-------------------|
| `AgeFilter` | Posts older than threshold (~48 hours) |
| `DropDuplicatesFilter` | Duplicate post IDs |
| `CoreDataHydrationFilter` | Posts that failed metadata fetch |
| `SelfTweetFilter` | Your own posts |
| `RepostDeduplicationFilter` | Multiple retweets of same content |
| `IneligibleSubscriptionFilter` | Paywalled content you can't access |
| `PreviouslySeenPostsFilter` | Posts you've already seen (Bloom filter) |
| `PreviouslyServedPostsFilter` | Posts already served in session |
| `MutedKeywordFilter` | Posts containing your muted keywords |
| `AuthorSocialgraphFilter` | Posts from blocked/muted accounts |

### Post-Selection Filters (Final Safety Check)

| Filter | What Gets Removed |
|--------|-------------------|
| `VFFilter` | Deleted/spam/violence/gore/safety-flagged |
| `DedupConversationFilter` | Multiple branches of same conversation |

---

## User Timeline Optimization

### For Readers (Improving Your Feed)

The algorithm learns from your actions:

**Actions That Train Your Feed:**
- Likes → More similar content
- Replies → Signals high interest in topic/author
- Clicks → Even without engagement, signals interest
- Dwell time → Spending time = implicit positive signal
- Profile clicks → Interest in author

**Actions That Filter Your Feed:**
- "Not Interested" → Tanks that post's score for you
- Mute keyword → Filters out topic entirely
- Mute account → Removes without blocking
- Block → Nuclear option, removes forever

**Key Insight:** The algorithm doesn't know you're angry. Hate-clicking still counts as engagement. Stop interacting with content you don't want more of.

### Training a Better Feed

1. Actively like/reply to content you want more of
2. Use "Not Interested" aggressively on bad content
3. Mute keywords for topics you're tired of
4. Stop hate-clicking (engagement = more of it)
5. Follow better accounts (in-network priority)

---

## Technical Implementation Details

### User Action Sequence
Your recent engagement history is fetched and aggregated:
- Window time limit on recent actions
- Maximum sequence length (truncated to last N items)
- Used by both retrieval and ranking models

### Hash-Based Embeddings
Both user and content use multiple hash functions for embedding lookup:
- User hashes
- Item (post) hashes  
- Author hashes

### Post Store Retention
Thunder maintains posts for 2-day retention period:
- Posts older than ~48 hours trimmed
- Deleted posts tracked and filtered
- Per-author limits on stored posts

### Request Timeout
Post retrieval has timeout protection to prevent slow responses when fetching from many followed accounts.
