import requests
import os
from urllib.parse import urlparse

def fetch_image():
    try:
        image_url = input("Enter the image URL: ").strip()
        if not image_url:
            print("No URL provided. Exiting.")
            return

        folder_name = "Fetched_Images"
        os.makedirs(folder_name, exist_ok=True)

        parsed_url = urlparse(image_url)
        filename = os.path.basename(parsed_url.path)
        if not filename:
            filename = "image_downloaded.jpg"

        filepath = os.path.join(folder_name, filename)

        response = requests.get(image_url)
        response.raise_for_status()

        with open(filepath, "wb") as f:
            f.write(response.content)

        print(f"Image saved successfully as {filepath}")

    except requests.exceptions.MissingSchema:
        print("Invalid URL. Include 'http://' or 'https://'.")
    except requests.exceptions.HTTPError as http_err:
        print(f"HTTP error occurred: {http_err}")
    except requests.exceptions.ConnectionError:
        print("Failed to connect. Check your internet.")
    except Exception as err:
        print(f"An error occurred: {err}")

if __name__ == "__main__":
    fetch_image()
