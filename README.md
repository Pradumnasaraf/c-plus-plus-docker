A simple HTTP server implemented in C++ using the C++ REST SDK. It listens for incoming HTTP GET requests and responds with a JSON message. This is for Docker's [C++ Language Guide](https://docs.docker.com/language/cpp/).

## API

The server only supports the HTTP GET method at the moment. When a GET request is received, the server responds with a JSON object:

```json
{
    "message": "OK"
}
```

## Running with Docker Compose

Below is the [Dockerfile](Dockerfile) for the C++ application:

```Dockerfile
# Use the official Ubuntu image as the base image
FROM ubuntu:26.04

# Set the working directory in the container
WORKDIR /app

# Install necessary dependencies
RUN apt-get update && apt-get install -y \
    g++ \
    libcpprest-dev \
    libboost-all-dev \
    libssl-dev \
    cmake

# Copy the source code into the container
COPY ok_api.cpp .

# Compile the C++ code
RUN g++ -o ok_api ok_api.cpp -lcpprest -lboost_thread -lboost_chrono -lboost_random -lssl -lcrypto

# Expose the port on which the API will listen
EXPOSE 8080

# Command to run the API when the container starts
CMD ["./ok_api"]
```

To run this application using Docker Compose, you'll need to create a `compose.yml` file.

Here's the `compose.yml` file:

```yaml
services:
  ok-api:
    image: ok-api
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "8080:8080"
```

To build and run the Docker image using Docker Compose, use the following command:

```bash
docker-compose up
```

This will build the Docker image and then run it, mapping the container's port 8080 to port 8080 on the host machine. You can then access the API by visiting `http://localhost:8080` in your web browser.

## Contributing

Any feedback and contributions are welcome! Please open an issue before submitting a pull request.

## License

[MIT License](LICENSE)
