# Capstone Travel ML API
This is a repository containing the deployment of the machine learning model used by the backend API. The model runs in Python and is deployed using a Flask web server listening on port 5000.


## Disclaimer
You can provide a `.env` file to set an API key for the model. Just provide an `API_KEY` environmental variable and the API will automatically use it to validate incoming requests.
All requests is validated through a `key` query param in the url like so [https://modelapiurlhere.com?key=this+api+key](https://modelapiurlhere.com?key=this+api+key)

> [!IMPORTANT]
> If no `API_KEY` environmental variable is provided, the server will launch without validating requests.


## API Documentation
This assumes that the API is running on [http://localhost:5000](http://localhost:5000), if your deployed API is in another location then feel free to change the url.

### Check (GET)

    http://localhost:5000/check

This always returns a 200 HTTP response, use it to check if the server is up or not. The response schema is as follows:
```
{
    message: "api is up and running"
}
```

### Recommend (POST)

    http://localhost:5000/recommend

This endpoint receives a POST request with a json payload and returns the model output from that payload. The payload schema is as follows:
```
{
    "text": "Saya ingin pergi ke tempat yang berhubungan dengan sejarah dan budaya.",
    "city": "Jakarta",
    "rating_preference": "4 stars and above",
    "review_preference": "More than 20 reviews"
}
```

- text: what text describes the travel location
- city: the city in Indonesia that the destination is located at
- rating_preference: the preferred rating of the location
- review_preference: the preferred number of reviews

> [!IMPORTANT]
> The `text` property is **mandatory**, but the other fields are optional.

The endpoint then returns an json object with the following schema:

```
{
    "class_label": "Culture",
    "location_count": 8,
    "recommendations": [
        {
            "Categories": "Culture",
            "Categories_Label": 0,
            "City": "Jakarta",
            "Coordinate": "{'lat': -6.137644799999999, 'lng': 106.8171245}",
            "Description": "The old city in Jakarta, which is also called Kota Tua, is centered on Alun-Alun Fatahillah, which is a busy square with routine performances of traditional dances. The Jakarta History Museum is a Dutch-era building with paintings and antiques, while the Wayang Museum displays Javanese wooden puppets. Glodok Village, or Chinatown, is famous for its street food, such as dumplings and fried noodles. Nearby, there are schooners and fishing boats in the quaint Sunda Kelapa harbor",
            "Lat": -61376448,
            "Long": 1068171245,
            "Place_Name": "Kota Tua",
            "Price": 0,
            "Rating_Count": 25,
            "Ratings": 46,
            "Time_Minutes": 90.0,
            "_1": 2
        },
        .
        .
        .
    ]
}
```

## Local Installation
Clone the repository

    git clone https://github.com/entertainmeproject/ml-api-travel.git

Move into the directory

    cd ml-api-travel

Run the application

    python main.py

The machine learning model API will run on [http://localhost:5000](http://localhost:5000) by default.

## Containerize The Application
Run the following command in the directory where the model API is located.

    docker build -t container_tag .

Run the built Docker image

## GCP Installation
Clone the repository

    git clone https://github.com/entertainmeproject/ml-api-travel.git

Move into the directory

    cd ml-api-travel

Containerize the application as follows. 
> [!NOTE]
> Replace anything fully capitalized and preceded with a `$` sign with the appropriate names

    docker build -t $REGION-docker.pkg.dev/$GCP_PROJECT/$ARTIFACT_REGISTRY_REPO/$IMAGE:$TAG .

Make sure that you've created a repository in artifact registry inside the google cloud project that you can use to store the docker images.

Afterwards, push the image to artifact registry with

    docker push $REGION-docker.pkg.dev/$GCP_PROJECT/$ARTIFACT_REGISTRY_REPO/$IMAGE:$TAG

If everything is successful, you should be able to deploy the ML API to Cloud Run. Assign at least 1GB of RAM to the instance so it runs smoothly.
