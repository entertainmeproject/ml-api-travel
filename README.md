# Capstone Travel/Tour ML API
This is a repository containing the deployment of the machine learning model used by the backend API. The model runs in Python and is deployed using a Flask web server listening on port 5000.


## Disclaimer
You can provide a `.env` file to set an API key for the model. Just provide an `API_KEY` environmental variable and the API will automatically use it to validate incoming requests.
All requests is validated through a `key` query param in the url like so [https://modelapiurlhere.com?key=this+api+key](https://modelapiurlhere.com?key=this+api+key)

If no `API_KEY` environmental variable is provided, the server will launch without validating requests.


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

## Google Cloud Installation
Clone the repository

    git clone https://github.com/entertainmeproject/ml-api-travel.git

Move into the directory

    cd ml-api-travel

Containerize the application as follows
