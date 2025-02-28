import requests
import matplotlib.pyplot as plt

# GitHub username (replace with your actual GitHub username)
USERNAME = "CAPTAINCODERCOOL"

# GitHub API URL to fetch repositories
url = f"https://api.github.com/users/{USERNAME}/repos"
response = requests.get(url)

# Check for API request success
if response.status_code == 200:
    repos = response.json()
    
    # Dictionary to store language usage count
    language_counts = {}

    # Iterate over repositories to count language usage
    for repo in repos:
        if repo.get("language"):  # Check if language data is available
            language = repo["language"]
            language_counts[language] = language_counts.get(language, 0) + 1

    # Sort languages by frequency
    sorted_languages = sorted(language_counts.items(), key=lambda x: x[1], reverse=True)

    # Generate a bar chart for visualization
    if sorted_languages:
        plt.figure(figsize=(10, 5))
        plt.barh([lang[0] for lang in sorted_languages], [lang[1] for lang in sorted_languages], color='skyblue')
        plt.xlabel("Number of Repositories")
        plt.ylabel("Programming Language")
        plt.title(f"Most Used Languages in {USERNAME}'s GitHub Repositories")
        plt.gca().invert_yaxis()  # Reverse order for better visibility
        plt.show()
    else:
        print("No language data found in the repositories.")
else:
    print(f"Failed to fetch repositories. HTTP Status Code: {response.status_code}")
