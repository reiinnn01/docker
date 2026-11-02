# Django Application – Docker Deployment

## Project Description

This project demonstrates how to containerize and deploy a Django application using Docker. Docker allows the application to run consistently across different machines without requiring users to manually install Python dependencies or configure the development environment.

The application can be built into a Docker image, uploaded to Docker Hub, and downloaded and executed on another machine.

## Project Structure

```text
django-app/
│
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── .dockerignore
├── README.md
│
└── for_client/
    └── docker-compose.yml
```

### File Descriptions

| File | Purpose |
|---|---|
| `Dockerfile` | Defines the instructions for building the Django application image. |
| `docker-compose.yml` | Configures and builds the Docker image on the development machine. |
| `requirements.txt` | Contains the Python dependencies required by the application. |
| `.dockerignore` | Excludes unnecessary files and directories from the Docker build context. |
| `for_client/docker-compose.yml` | Allows another machine to download and run the published Docker image from Docker Hub. |
| `README.md` | Provides project documentation and deployment instructions. |

---

## Prerequisites

Before running the application, make sure the following are installed:

- Docker
- Docker Compose (included in modern Docker Desktop installations)
- A Docker Hub account (for publishing and downloading the image)

Docker installation:
- **Windows/Mac:** Download Docker Desktop from https://www.docker.com/products/docker-desktop/
- **Linux:** Install Docker Engine and the Docker Compose plugin.

---

## Part I – Building the Docker Image

This section explains how to build the application image on the development machine.

### Step 1: Build the Image

Open a terminal in the project directory and execute:

```bash
docker compose build
```

Alternatively, you can build the image directly using:

```bash
docker build -t yourusername/django-app:latest .
```

Replace `yourusername` with your Docker Hub username.

### Step 2: Verify the Image

Check whether the image was successfully created:

```bash
docker images
```

### Step 3: Run the Application Locally

Start the application using Docker Compose:

```bash
docker compose up -d
```

Open the application in your browser:

```text
http://localhost:8000
```

To stop the application:

```bash
docker compose down
```

---

## Part II – Publishing the Image to Docker Hub

To make the application available on other machines, the Docker image must first be uploaded to Docker Hub.

### Step 1: Log in to Docker Hub

```bash
docker login
```

Enter your Docker Hub credentials when prompted.

### Step 2: Tag the Image

```bash
docker tag yourusername/django-app:latest yourusername/django-app:latest
```

If the image was already built using the correct name, this step is unnecessary.

### Step 3: Push the Image

```bash
docker push yourusername/django-app:latest
```

Docker will upload the image layers to your Docker Hub repository.

After successfully pushing, the image will be available for download.

---

## Part III – Installing on Another Machine

This section explains how another user or computer can run the application without building the image again.

### Prerequisites

Make sure Docker is installed on the target machine.

### Step 1: Pull the Image

Open a terminal and execute:

```bash
docker pull yourusername/django-app:latest
```

Docker will download the application image from Docker Hub.

### Step 2: Prepare the Configuration File

Navigate to the `for_client` directory and use the provided `docker-compose.yml`.

This configuration uses `image:` instead of `build:` because the application image is already available on Docker Hub.

Make sure the image name matches the one uploaded to Docker Hub.

### Step 3: Start the Application

Inside the `for_client` directory, execute:

```bash
docker compose up -d
```

Docker Compose will create and start the application container.

### Step 4: Access the Application

Open a browser and navigate to:

```text
http://localhost:8000
```

The Django application should now be accessible.

### Step 5: Manage the Container

To check the running containers:

```bash
docker compose ps
```

To view application logs:

```bash
docker compose logs -f
```

To stop the application:

```bash
docker compose down
```

---

## Docker Deployment Workflow

The deployment process follows these steps:

1. Develop the Django application.
2. Create a Dockerfile and configure the Docker Compose file.
3. Build the Docker image.
4. Test the application locally.
5. Log in to Docker Hub.
6. Push the Docker image to Docker Hub.
7. On another machine, pull the published image.
8. Use the client Docker Compose configuration to start the application.
9. Access the application through a web browser.

---

## Notes

- Replace `yourusername` with the actual Docker Hub username.
- Ensure that the Docker image has been successfully pushed before attempting to pull it on another machine.
- The client configuration uses the published image and does not require a local Dockerfile.
- SQLite data persistence depends on the volume configuration in the client Docker Compose file.
- The application uses port `8000` by default.
- If port `8000` is already occupied, change the host-side port mapping in the Docker Compose configuration.
