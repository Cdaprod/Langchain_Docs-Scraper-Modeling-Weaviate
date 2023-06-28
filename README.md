# Langchain - Documentation Scraper
### by: David Cannan

This repository contains a script and associated files for scraping web-based documentation, formatting it for compatibility with the Langchain language model, and loading it into a local Weaviate database. This enables you to train a model on the scraped documentation and use it in conjunction with Langchain's ChatDB for conversational purposes.

## Getting Started

These instructions will guide you through the process of setting up your local environment and running the script.

### Prerequisites

You'll need Docker and Python 3.8 or newer installed on your machine to run this project.

### Installation

1. Clone this repository to your local machine.
2. Navigate to the project directory.
3. Run `docker-compose up -d` to start the Weaviate instance.
4. Install Python dependencies by running `pip install -r requirements.txt`.

### Usage

Once the Weaviate instance is up and running and Python dependencies are installed, you can run the script using the command `python doc_scraper.py`.

## Components

* `app.py`: This is the main script. It scrapes web-based documentation, formats it for compatibility with Langchain, and loads it into Weaviate.
* `requirements.txt`: This file lists the Python dependencies required by the script.
* `docker-compose.yml`: This file is used to start the Weaviate instance.

## Support

For questions, issues, or assistance with this project, please open an issue on this GitHub repository.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.



---
# Building docs




The code provided earlier assumes that you have a running Weaviate instance available at the specified URL (`localhost:8080`), and it requires the `weaviate-client` Python package to connect to and interact with the Weaviate instance.

To run this code locally, you need to have Weaviate set up and running on your local machine. This involves:

1. Installing Docker: Weaviate runs in a Docker container, so you will need Docker installed on your machine. Docker can be downloaded from the official Docker website.

2. Running a Weaviate instance: Once Docker is installed, you can start a Weaviate instance using a Docker command provided in the Weaviate documentation. This will start a local Weaviate instance that can be accessed at `localhost:8080`.

3. Installing `weaviate-client`: This can be done using pip, Python's package manager. You can install it by running `pip install weaviate-client` in your terminal.

The code also assumes that you have the required Python packages installed, which include:

- `langchain`: the language chain library for creating language models
- `requests`: for making HTTP requests
- `beautifulsoup4`: for parsing HTML and extracting information
- `pandas`: for data manipulation and analysis

These packages can be installed using pip, e.g., `pip install langchain requests beautifulsoup4 pandas`.

Once you have these prerequisites set up, you should be able to run the script on your local machine. Please ensure that you replace any placeholders (like `{your_weaviate_instance}`) with the appropriate values for your setup.

---

Sure, you can use Docker Compose to run multiple services together. Here's an example of a `docker-compose.yml` file which includes both Weaviate and a Python service:

```yaml
version: '3.4'
services:
  weaviate:
    image: semitechnologies/weaviate:latest
    ports:
      - 8080:8080
    environment:
      - WEAVIATE_HOST=localhost
      - WEAVIATE_PORT=8080
      - WEAVIATE_SCHEME=http
      - WEAVIATE_AUTHENTICATION_ANONYMOUS_ACCESS_ENABLED=true

  python-app:
    build: .
    volumes:
      - .:/app
    environment:
      - WEAVIATE_HOST=weaviate
      - WEAVIATE_PORT=8080
      - WEAVIATE_SCHEME=http
    depends_on:
      - weaviate
```

And here's a Dockerfile for your Python application:

```Dockerfile
# Use an official Python runtime as a parent image
FROM python:3.9-slim-buster

# Set the working directory in the container to /app
WORKDIR /app

# Add the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Run app.py when the container launches
CMD ["python", "app.py"]
```

In this configuration, the `python-app` service uses the same network as the `weaviate` service, so it can access Weaviate at `http://weaviate:8080`. The `depends_on` option is used to make sure that the `weaviate` service is started before the `python-app` service.

You can build and start the services using the command: `docker-compose up --build`

Remember to replace `"app.py"` in the CMD line with the actual name of your Python script. Also, the Dockerfile assumes that your Python script and the `requirements.txt` file are in the same directory as the Dockerfile. If they are in a different directory, you'll need to adjust the `ADD` command accordingly.

---

Sure, here is a `requirements.txt` file that you can use to install the necessary Python packages for your script:

```text
beautifulsoup4==4.10.0
requests==2.26.0
langchain==<version>
redis==3.5.3
weaviate-client==2.5.0
```

You can install these requirements using the command: `pip install -r requirements.txt`

Make sure to replace `<version>` with the version of `langchain` you are using.

Please note that the versions of the packages in the `requirements.txt` file are as of my knowledge cutoff in September 2021, and you might want to check for the latest versions of these packages. Also, the `requirements.txt` only includes the main packages needed to run your script and does not include their dependencies. When you run the `pip install -r requirements.txt` command, pip will also install the dependencies of these packages.

Remember, to run this environment locally, you'll need Python
