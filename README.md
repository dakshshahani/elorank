# elorank
### 1. **The Elo System Basics**

The Elo rating system was originally designed for chess, but it generalizes well to any 1-vs-1 "matchup."  
For each item AA with rating RAR_A and item BB with rating RBR_B:

1. **Expected score of A (probability A should win):**

$$
E_{A} = \frac{1}{ 1+ 10^{(R_{B}-R_{A})/400}}​
$$
Similarly,

$$
E_{B} = \frac{1}{ 1+ 10^{(R_{A}-R_{B})/400}}​
$$


Notice: $$
E_{A} + E_{B} = 1
$$

2. **Update after a match:**  
    If $S_{A}$ is the actual outcome for A (1 = win, 0 = loss, 0.5 = tie):

$$
R'_{A} = R_{A} + K \cdot(S_{A} - E_{A})
$$
$$
R'_{B} = R_{B} + K \cdot(S_{B} - E_{B})
$$
Where:
- $K$ = sensitivity constant (controls how fast ratings change). Common choices: 16, 24, 32.
- If A wins: $S_{A}=1, S_{B}=0$
- If B wins: $S_{B}=1, S_{A}=0$

---
### 2. **How This Applies to My App**

Each **vote** is equivalent to a "match."
- Voter picks which content they like better.
- Winner gets Elo boost, loser loses Elo.
- After many matches, ratings converge to reflect the **relative quality** as perceived by voters.

Example:
- Photo A (1200 Elo) vs Photo B (1000 Elo).
- Expected outcome: A should win most of the time.
- If B wins, B’s rating increases significantly, and A’s rating decreases more than usual.

This makes surprising wins "worth more," which prevents strong items from being overrated.

---

### 3. **Scaling With More Votes**

Elo gets **more accurate as the number of votes increases**:
- **Few votes** → Ratings are unstable and can swing wildly.
- **Many votes** → Ratings stabilize, variance decreases, leaderboard is more reliable.

This happens because:

- $E_A$ models the **expected probability of winning**. Over time, repeated matches converge actual win-rates to expected ones.
- Items with inflated/deflated scores naturally drift toward their “true” rating after enough comparisons.

Mathematically: the more comparisons, the smaller the average error between Elo and real underlying preference.

---

### 4. **Improving the Formula for Large Scale**

Elo is simple, but you can adapt it:

- **Dynamic K-factor**: Reduce K as the number of matches grows (so early votes have bigger impact, later votes stabilize ratings).
    where nn = number of matches, cc = constant scaling factor.
    
- **Bayesian Elo (Glicko, TrueSkill)**: These are extensions of Elo that also model **uncertainty** in ratings. They’re great if your app grows big, because they give confidence intervals (e.g., "This photo’s true score is ~1500 ± 100").
    
- **Pairing Strategy**: Elo works best if you **don’t just randomly pair items**. Instead, you can:
    
    - Pair items of similar Elo (like in chess tournaments).
        
    - Occasionally pair with higher/lower Elo to calibrate.
        

---

### 5. **Leaderboards**

- Sort items by Elo rating.
    
- Optionally display confidence ranges if you use Glicko/TrueSkill.
    
- You can add filters (by category, type of content) to generate multiple leaderboards.
    

---

✅ **Summary in Plain Words:**

- Each vote = a match.
    
- Expected outcome depends on the Elo difference.
    
- Ratings adjust so that "surprising wins" count more.
    
- With more votes, ratings converge to stable rankings.
    
- You can improve accuracy with dynamic K or advanced systems like Glicko/TrueSkill.
    
