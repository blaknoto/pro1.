# pro1.

pip install requests
import requests
import re

def get_username_from_url(url):
    match = re.match(r'https://github\.com/([a-zA-Z0-9_-]+)', url)
    if match:
        return match.group(1)
    else:
        raise ValueError("Invalid GitHub profile URL")

def fetch_repositories(username):
    url = f'https://api.github.com/users/{username}/repos'
    response = requests.get(url)
    
    if response.status_code == 200:
        return response.json()
    else:
        response.raise_for_status()

def display_repositories(repos):
    if not repos:
        print("No public repositories found.")
    else:
        for repo in repos:
            print(f"Name: {repo['name']}")
            print(f"URL: {repo['html_url']}")
            print(f"Description: {repo.get('description', 'No description')}")
            print('-' * 40)

def main():
    profile_url = input("Enter GitHub profile URL: ")
    
    try:
        username = get_username_from_url(profile_url)
        print(f"Fetching repositories for user: {username}")
        
        repos = fetch_repositories(username)
        display_repositories(repos)
    
    except ValueError as e:
        print(e)
    except requests.RequestException as e:
        print(f"An error occurred: {e}")

if __name__ == "__main__":
    main()
