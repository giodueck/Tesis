# Getting an auth token for Copernicus data downloads

```
curl --location --request POST 'https://identity.dataspace.copernicus.eu/auth/realms/CDSE/protocol/openid-connect/token' \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'grant_type=password' \
  --data-urlencode 'username=<LOGIN>' \
  --data-urlencode 'password=<PASSWORD>' \
  --data-urlencode 'client_id=cdse-public'
```

This returns a Refresh Token as well, which can be used to get a new Access Token without specifying Login and Password:
```
curl --location --request POST 'https://identity.dataspace.copernicus.eu/auth/realms/CDSE/protocol/openid-connect/token' \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'grant_type=refresh_token' \
  --data-urlencode 'refresh_token=<REFRESH_TOKEN>' \
  --data-urlencode 'client_id=cdse-public'
```

Once you have your token, you require a product Id which can be found in the response of the products search:
https://catalogue.dataspace.copernicus.eu/odata/v1/Products

Download using following commands:

> [!Tip] The examples below assume that the product is saved to a file with the “.zip” extension. The exceptions are Sentinel-5P products, which are served directly with the “.nc” extension.

```
curl -H "Authorization: Bearer $ACCESS_TOKEN" 'https://catalogue.dataspace.copernicus.eu/odata/v1/Products(060882f4-0a34-5f14-8e25-6876e4470b0d)/$value' --location-trusted --output /tmp/product.zip
```

or

```
wget  --header "Authorization: Bearer $ACCESS_TOKEN" 'https://catalogue.dataspace.copernicus.eu/odata/v1/Products(db0c8ef3-8ec0-5185-a537-812dad3c58f8)/$value' -O example_odata.zip
```

or

```python
import requests

# Make sure access_token is defined
access_token = "your_access_token"  # Replace with your actual access token

url = f"https://download.dataspace.copernicus.eu/odata/v1/Products(a5ab498a-7b2f-4043-ae2a-f95f457e7b3b)/$value"

headers = {"Authorization": f"Bearer {access_token}"}

# Create a session and update headers
session = requests.Session()
session.headers.update(headers)

# Perform the GET request
response = session.get(url, stream=True)

# Check if the request was successful
if response.status_code == 200:
    with open("product.zip", "wb") as file:
        for chunk in response.iter_content(chunk_size=8192):
            if chunk:  # filter out keep-alive new chunks
                file.write(chunk)
else:
    print(f"Failed to download file. Status code: {response.status_code}")
    print(response.text)
```
