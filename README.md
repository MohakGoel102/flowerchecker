[Plant.id](https://web.plant.id) offers [plant identification](https://web.plant.id/plant-identification-api/) and [plant diseases identification](https://web.plant.id/plant-health-assessment/) API based on machine learning. Once you [obtain the API key](https://web.plant.id/plant-identification-api/), you can use these client's code available in this repository to speed-up the development of your implementation.

# Plant.id API v2

See our **[documentation](https://github.com/flowerchecker/Plant-id-API/wiki)** for the full reference.

Don't know how to code? Try the [online demo](https://plant.id/). 🌐

## Plant Identification 🌱

Send us your plant images encoded in base64, and get a list of possible species suggestions with additional information.

```python
import base64
import requests

# encode images to base64
with open("unknown_plant.jpg", "rb") as file:
    images = [base64.b64encode(file.read()).decode("ascii")]

response = requests.post(
    "https://api.plant.id/v2/identify",
    json={
        "images": images,
        "modifiers": ["similar_images"],
        "plant_details": ["common_names", "url"],
    },
    headers={
        "Content-Type": "application/json",
        "Api-Key": "-- ask for one: https://web.plant.id/api-access-request/ --",
    }).json()

for suggestion in response["suggestions"]:
    print(suggestion["plant_name"])    # Taraxacum officinale
    print(suggestion["plant_details"]["common_names"])    # ["Dandelion"]
    print(suggestion["plant_details"]["url"])    # https://en.wikipedia.org/wiki/Taraxacum_officinale
```

Response example: [response_species_identification.json](https://github.com/flowerchecker/Plant-id-API/blob/master/response_species_identification.json).

## Health Assessment 🥀

Send us your ill plant photos encoded in base64, and get a list of possible health issues your plant suffers from.

```Python
import base64
import requests

# encode images to base64
with open("ill_plant.jpg", "rb") as file:
    images = [base64.b64encode(file.read()).decode("ascii")]

response = requests.post(
    "https://api.plant.id/v2/health_assessment",
    json={
        "images": images,
        "modifiers": ["similar_images"],
        "disease_details": ["description", "treatment"],
    },
    headers={
        "Content-Type": "application/json",
        "Api-Key": "-- ask for one: https://web.plant.id/api-access-request/ --",
    }).json()

if not response["health_assessment"]["is_healthy"]:
    for suggestion in response["health_assessment"]["diseases"]:
        print(suggestion["name"])   # water deficiency
        print(suggestion["disease_details"]["description"])    # Water deficiency is...
```

Response example: [response_health_assessment.json](https://github.com/flowerchecker/Plant-id-API/blob/master/response_health_assessment.json).
