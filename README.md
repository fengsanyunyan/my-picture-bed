# my-picture-bed
used as my personal picture bed

**From chatGPT**
> To upload an image from your local machine to a specific repository on Github and get the uploaded image URL, you can use the Github API and Python's requests library. Here's an example code snippet that demonstrates how to do this:
```python
import requests
import base64

# Github username and personal access token
username = 'your_username'
token = 'your_token'

# Repository details
repo_owner = 'owner_username'
repo_name = 'repository_name'
repo_path = 'images/image.jpg'

# Image file path
file_path = '/path/to/local/image.jpg'

# Read image file as bytes
with open(file_path, 'rb') as file:
    file_content = file.read()

# Base64 encode image content
encoded_content = base64.b64encode(file_content)

# Create API endpoint URL
url = f'https://api.github.com/repos/{repo_owner}/{repo_name}/contents/{repo_path}'

# Create request headers
headers = {
    'Content-Type': 'application/json',
    'Authorization': f'token {token}',
    'User-Agent': username
}

# Create request body
data = {
    'message': 'Upload image',
    'content': encoded_content.decode('utf-8')
}

# Send POST request to upload image
response = requests.post(url, json=data, headers=headers)

# Get uploaded image URL from response
image_url = response.json()['content']['html_url']
```
> Here, you'll need to replace the following placeholders with your own information:
> 
> username: Your Github username
> + **`token`**: A personal access token with the repo scope that you've generated on Github
> + **`repo_owner`**: The username of the owner of the repository
> + **`repo_name`**: The name of the repository
> + **`repo_path`**: The path where you want to upload the image, relative to the repository root
> + **`file_path`**: The path of the image file on your local machine
> 
> The code first reads the image file as bytes, and then base64 encodes the content. It then creates the API endpoint URL, request headers, and request body. Finally, it sends a POST request to upload the image, and extracts the uploaded image URL from the response.
> 
> Note that this code assumes that the repository is public. If the repository is private, you'll need to add the repo scope to your personal access token, and modify the request headers to include the Accept header with the value 'application/vnd.github.v3+json'.

Then I ask chatGPT to write above code into function, the answer is:
> Sure, here's the same code refactored into a function:
```python
import requests
import base64

def upload_image_to_github(file_path, repo_owner, repo_name, repo_path, username, token):
    # Read image file as bytes
    with open(file_path, 'rb') as file:
        file_content = file.read()

    # Base64 encode image content
    encoded_content = base64.b64encode(file_content)

    # Create API endpoint URL
    url = f'https://api.github.com/repos/{repo_owner}/{repo_name}/contents/{repo_path}'

    # Create request headers
    headers = {
        'Content-Type': 'application/json',
        'Authorization': f'token {token}',
        'User-Agent': username
    }

    # Create request body
    data = {
        'message': 'Upload image',
        'content': encoded_content.decode('utf-8')
    }

    # Send POST request to upload image
    response = requests.post(url, json=data, headers=headers)

    # Get uploaded image URL from response
    image_url = response.json()['content']['html_url']
    return image_url
```
> You can now call this function and pass in the required arguments, like so:
```python
username = 'your_username'
token = 'your_token'
repo_owner = 'owner_username'
repo_name = 'repository_name'
repo_path = 'images/image.jpg'
file_path = '/path/to/local/image.jpg'

image_url = upload_image_to_github(file_path, repo_owner, repo_name, repo_path, username, token)

print(f'Uploaded image URL: {image_url}')
```
> The function takes the same arguments as the previous code, and returns the uploaded image URL.
