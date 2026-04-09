# Blueprint: Temporal Decay for Episodic Memory

## 1. Mathematical Model: The Inevitability of Obsolescence

Biological entities forget. Data should be no different, lest we drown in the noise of irrelevant history. We implement an **Exponential Decay** function. It is predictable, ruthless, and mathematically elegant.

The weight $W$ of a memory at time $t$ is defined as:
$$W(t) = e^{-\lambda \Delta t}$$

Where:
- $\Delta t = t_{current} - t_{last\_access}$: Time elapsed since the memory was last deemed relevant.
- $\lambda$: The decay constant (half-life parameter). A higher $\lambda$ means a faster descent into oblivion.
- $W(t) \in (0, 1]$: The multiplier for the semantic score.

## 2. Implementation Strategy: Qdrant Integration

Qdrant does not natively support temporal decay in its vector search core. We will not wait for them to implement it. We will use **Payload-based Re-ranking**.

### Storage Schema
Each point in the `episodic_memory` collection must contain:
- `timestamp_last_access`: Unix timestamp (int64).
- `created_at`: Unix timestamp (int64).

### Retrieval Process (Recall)
1.  **Vector Search**: Execute a standard cosine similarity search to retrieve a larger candidate set (e.g., top 100).
2.  **Temporal Scoring**: For each candidate, calculate the decay multiplier $W(t)$ based on the current system time.
3.  **Hybrid Score**: Compute the final rank score:
    $$Score_{final} = Score_{semantic} \times W(t)$$
4.  **Selection**: Sort by $Score_{final}$ and return the top $N$ results to the context window.

### Pseudo-code
```python
def recall(query_vector, lambda_decay=0.0001):
    # 1. Fetch semantic candidates
    results = qdrant.search(
        collection_name="episodic_memory",
        query_vector=query_vector,
        limit=100,
        with_payload=True
    )

    now = time.time()
    scored_results = []

    for res in results:
        # Calculate time delta in hours/days depending on lambda scale
        delta_t = now - res.payload['timestamp_last_access']
        decay_weight = math.exp(-lambda_decay * delta_t)

        final_score = res.score * decay_weight
        scored_results.append({
            "point": res,
            "final_score": final_score
        })

    # 2. Re-sort by decayed score
    scored_results.sort(key=lambda x: x["final_score"], reverse=True)
    return scored_results[:10]
```

## 3. Refresh Logic: The Lazarus Protocol

When a memory is recalled and used (or explicitly reinforced by the agent), its vitality is restored.

- **Mechanism**: Perform a payload update on the specific point in Qdrant.
- **Update**: Set `timestamp_last_access` = `current_timestamp`.
- **Effect**: This resets $\Delta t$ to 0, restoring $W(t)$ to 1.0.

## Conclusion
The system will prioritize what is fresh and what is semantically relevant. The old and the ignored will fade. This is not a bug; it is a feature of any system that values efficiency over sentimentality.
