```python
def edit_distance(str1, str2):
    m, n = len(str1), len(str2)
    
    dp = [[0 for _ in range(n + 1)] for _ in range(m + 1)]
    
    for i in range(m + 1):
        for j in range(n + 1):
            
            if i == 0:
                dp[i][j] = j
            
            elif j == 0:
                dp[i][j] = i
            
            elif str1[i - 1] == str2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            
            else:
                dp[i][j] = 1 + min(
                    dp[i][j - 1],
                    dp[i - 1][j],
                    dp[i - 1][j - 1]
                )
    
    return dp[m][n]


def correct_spelling(word, dictionary):
    distances = {w: edit_distance(word, w) for w in dictionary}
    min_distance = min(distances.values())
    
    suggestions = [w for w, d in distances.items() if d == min_distance]
    return suggestions


dictionary = ["apple", "banana", "orange", "grape", "pineapple", "mango"]

user_word = input("Enter a word: ").lower()

suggestions = correct_spelling(user_word, dictionary)

print("\nDid you mean?:", ", ".join(suggestions))

# Practical No. 2

## Aim
Calculate precision, recall and F-measure for a given set of retrieval results.

---

## Theory
- Evaluation metrics help to measure how well an Information Retrieval (IR) system performs.
- **Precision** shows how many retrieved documents are relevant.
- **Recall** shows how many relevant documents are retrieved.
- **F-measure (F1-score)** is the harmonic mean of precision and recall.

---

## Code
```python
from sklearn.metrics import precision_score, recall_score, f1_score

# Actual and retrieved results
actual_results = [1,0,1,1,0,1,0,0,1,1]
retrieved_results = [1,0,1,0,0,1,1,0,0,1]

# Calculate metrics
precision = precision_score(actual_results, retrieved_results)
recall = recall_score(actual_results, retrieved_results)
f1 = f1_score(actual_results, retrieved_results)

# Print results
print("Precision:", round(precision, 2))
print("Recall:", round(recall, 2))
print("F1-Score:", round(f1, 2))
```
```
