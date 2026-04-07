'''def edit_distance(str1, str2):
    m, n = len(str1), len(str2)
    
    # Create DP table
    dp = [[0 for _ in range(n + 1)] for _ in range(m + 1)]
    
    for i in range(m + 1):
        for j in range(n + 1):
            
            if i == 0:
                dp[i][j] = j   # Insert all characters
            
            elif j == 0:
                dp[i][j] = i   # Remove all characters
            
            elif str1[i - 1] == str2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            
            else:
                dp[i][j] = 1 + min(
                    dp[i][j - 1],    # Insert
                    dp[i - 1][j],    # Remove
                    dp[i - 1][j - 1] # Replace
                )
    
    return dp[m][n]


def correct_spelling(word, dictionary):
    distances = {w: edit_distance(word, w) for w in dictionary}
    min_distance = min(distances.values())
    
    suggestions = [w for w, d in distances.items() if d == min_distance]
    return suggestions


# Dictionary words
dictionary = ["apple", "banana", "orange", "grape", "pineapple", "mango"]

# User input
user_word = input("Enter a word: ").lower()

# Get suggestions
suggestions = correct_spelling(user_word, dictionary)'''

print("\nDid you mean?:", ", ".join(suggestions))
