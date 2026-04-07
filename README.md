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
# 1 = Relevant, 0 = Not Relevant
actual_results = [1, 0, 1, 1, 0, 1, 0, 0, 1, 1]
retrieved_results = [1, 0, 1, 0, 0, 1, 1, 0, 0, 1]

TP = FP = FN = 0

for actual, retrieved in zip(actual_results, retrieved_results):
    if actual == 1 and retrieved == 1:
        TP += 1
    elif actual == 0 and retrieved == 1:
        FP += 1
    elif actual == 1 and retrieved == 0:
        FN += 1

precision = TP / (TP + FP) if (TP + FP) != 0 else 0
recall = TP / (TP + FN) if (TP + FN) != 0 else 0
f1_score = (2 * precision * recall / (precision + recall)) if (precision + recall) != 0 else 0

print("Precision:", round(precision, 2))
print("Recall:", round(recall, 2))
print("F1-Score:", round(f1_score, 2))
```

---

# Practical No. 3

## Aim
Use an evaluation toolkit to measure average precision and other evaluation metrics.

---

## Theory
- It measures how well a system ranks relevant documents for a single query.
- It averages the precision values obtained each time a relevant document is retrieved.

---

## Formula
Average Precision (AP):

AP = Σ (Precision@k × rel(k)) / Number of relevant documents

---

## Code
```python
import numpy as np

# Relevance list (1 = relevant, 0 = not relevant)
relevance = [1, 0, 1, 1, 0]

def average_precision(x):
    x = np.asarray(x)
    precisions = [np.mean(x[:k+1]) for k in range(len(x)) if x[k] == 1]
    return np.mean(precisions) if precisions else 0.0

# Calculate AP
AP = average_precision(relevance)

# Output
print("Average Precision (AP):", round(AP, 3))
```
```
---

# Practical No. 4

## Aim
Implement PageRank algorithm to rank web pages based on importance.

---

## Theory
- PageRank is an algorithm used to measure the importance of web pages.
- It works based on the number and quality of links pointing to a page.
- A page is important if it is linked by other important pages.
- Damping factor (usually 0.85) is used to simulate random browsing.

---

## Code
```python
def pagerank(graph, damping=0.85, iterations=10):
    pages = list(graph.keys())
    N = len(pages)

    # Initialize PageRank
    PR = {page: 1 / N for page in pages}

    for _ in range(iterations):
        new_PR = {}

        for page in pages:
            incoming_sum = 0

            for node in pages:
                if page in graph[node]:
                    incoming_sum += PR[node] / len(graph[node])

            new_PR[page] = (1 - damping) / N + damping * incoming_sum

        PR = new_PR

    return PR


# Example graph
graph = {
    'A': ['B', 'C'],
    'B': ['C'],
    'C': ['A'],
    'D': ['C']
}

# Calculate PageRank
ranks = pagerank(graph)

# Print results
for page, rank in ranks.items():
    print(f"{page}: {round(rank, 3)}")
```
'''WEB CRAWLER URL

import requests

# Simple web crawler that fetches HTML content
def crawl_urls(urls):
    pages = {}

    for url in urls:
        try:
            response = requests.get(url)
            pages[url] = response.text
            print(f"Fetched: {url}")
        except:
            print(f"Failed to fetch: {url}")

    return pages
    
urls = [
    "https://example.com",
    "https://www.python.org"
]


