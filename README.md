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
```
