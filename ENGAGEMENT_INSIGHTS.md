# X Algorithm: Key Insights on Maximizing Engagement

Based on analysis of the X (Twitter) recommendation algorithm codebase.

---

## How the Algorithm Rewards Content

### 1. The 18 Engagement Signals

The algorithm predicts and weights these actions:

#### High-Value Positive Signals

| Signal | What It Means |
|--------|---------------|
| `favorite` (Like) | High weight - easy engagement |
| `reply` | High weight - indicates conversation |
| `retweet` | High weight - amplification signal |
| `quote` | High weight - thoughtful engagement |
| `follow_author` | Strong signal you provide ongoing value |
| `dwell_time` | Time spent reading = quality indicator |

#### Medium-Value Signals

| Signal | What It Means |
|--------|---------------|
| `click` | Link clicks show interest |
| `profile_click` | Curiosity about you |
| `share` | DM/external sharing |
| `video_quality_view` | Watched video meaningfully |
| `photo_expand` | Engaged with media |

#### Negative Signals (Actively Penalized)

| Signal | Impact |
|--------|--------|
| `block_author` | **Large negative weight** |
| `report` | **Strong negative** |
| `mute_author` | Negative |
| `not_interested` | Negative |

---

## 2. Content Strategy Insights

### Optimize for Dwell Time

> The algorithm measures how long users spend on your post.

- Write content that takes time to read/process
- Use threads for longer narratives
- Include compelling hooks that make people stop scrolling

### Encourage Replies Over Likes

> Replies signal deeper engagement than passive likes.

- Ask questions
- Share opinions that invite discussion
- Create content that's easy to respond to

### Avoid Negative Actions at All Costs

> Block/mute/report have LARGE negative weights.

- Don't spam or post excessively (author diversity penalty kicks in)
- Avoid controversial content that triggers reports
- Don't engage in behavior that leads to mutes/blocks

### Video Strategy

```
MIN_VIDEO_DURATION_MS threshold exists
VQV_WEIGHT rewards quality video views
```

- Videos need to be watched for a minimum duration to count
- Optimize for watch-through, not just impressions
- Hook viewers in the first few seconds

---

## 3. The Author Diversity Penalty

```
2nd post from same author: score × 0.7
3rd post: score × 0.49
4th post: score × 0.34
Floor: never below 0.1× original
```

**Implications:**

- Posting many times in quick succession has diminishing returns
- Quality over quantity - one great post beats five mediocre ones
- Space out your content throughout the day

---

## 4. In-Network vs Out-of-Network

```
In-network (followers) content gets FULL score
Out-of-network content gets ~0.8-0.95× multiplier
```

**Strategy:**

- Building a follower base matters - your content gets preferential treatment for followers
- To reach non-followers, you need even stronger engagement signals to overcome the OON penalty
- Viral potential requires significantly higher predicted engagement

---

## 5. The History Sequence Matters

```
The model uses last ~128 user engagements
```

**What this means:**

- The algorithm learns from users' recent behavior
- If someone engages with your content, they're more likely to see future content
- Consistent engagement builds a "relationship" in the model's understanding

---

## 6. Recency Matters

```
MAX_POST_AGE filter removes posts older than ~7 days
```

- Content has a limited shelf life
- Fresh content is prioritized
- Evergreen content strategy doesn't work as well on X

---

## 7. Hash Embeddings = Content Patterns

The model uses hash-based embeddings for:

- **User hashes**: Your account's "fingerprint"
- **Post hashes**: Content patterns
- **Author hashes**: Your reputation signal

**Implication**: The algorithm learns patterns about what type of content you post and how users respond to it. Consistent quality builds a positive author embedding over time.

---

## Quick Reference: Engagement Maximization

| Do This | Avoid This |
|---------|------------|
| Write content that sparks replies | Posting content that gets ignored |
| Include media (photos/videos) | Text-only posts (less engagement surface) |
| Space posts throughout the day | Rapid-fire posting (diversity penalty) |
| Build genuine follower relationships | Chasing viral out-of-network reach |
| Create content worth dwelling on | Low-effort one-liners |
| Engage authentically | Behavior that triggers mutes/blocks |
| Post fresh content regularly | Letting your account go stale |
| Optimize video for watch-through | Videos people skip immediately |

---

## The Core Formula

```
Your Reach = Σ (P(positive_action) × weight) - Σ (P(negative_action) × weight)
           × author_diversity_factor
           × in_network_bonus
           × recency
```

**Bottom line**: Create content that makes people want to reply, share, and spend time with - while avoiding anything that makes them want to mute, block, or report you. The algorithm is essentially predicting "will this user have a positive or negative reaction?" and ranking accordingly.

---

## Algorithm Architecture Summary

For reference, the X algorithm uses a two-stage pipeline:

1. **Retrieval Stage**
   - Thunder: ~500 posts from followed accounts (in-network)
   - Phoenix Two-Tower Model: ~500 out-of-network posts

2. **Ranking Stage**
   - Phoenix Transformer predicts 18 engagement probabilities
   - Weighted combination into single score
   - Author diversity and in-network adjustments applied

3. **Filtering**
   - Age filter (~7 days max)
   - Deduplication
   - Blocked/muted author removal
   - Visibility filtering (spam, harmful content)

The model uses **zero hand-engineered features** - it learns entirely from user engagement sequences via a transformer architecture with candidate isolation.
