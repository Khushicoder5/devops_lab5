# Lab 5: Docker Security with AppArmor

## Objective
To secure Docker containers using AppArmor profiles and Python Docker SDK. This lab focuses on restricting access to sensitive directories, preventing unauthorized binary execution, and applying security options to a Docker container.

## Technologies Used
- Docker
- Python
- Flask
- AppArmor
- Docker SDK for Python
- WSL2
- Git and GitHub

## Project Files

- `app.py` – Flask web application.
- `Dockerfile` – Configuration to build the Docker image.
- `my-apparmor-profile` – AppArmor security profile.
- `apply_apparmor.py` – Python script to build the image, start the container, and configure the AppArmor security option.
- `test_restricted_actions.py` – Script for testing restricted actions, if included.
- `README.md` – Lab documentation.

## Tasks Completed

### 1. Create a Flask Application
Created a Flask application that runs on port 5000 inside the Docker container.

### 2. Build the Docker Image

```bash
docker build -t flask-apparmor .
```

### 3. Create the AppArmor Profile

Profile location:

```text
/etc/apparmor.d/my-apparmor-profile
```

The profile contains rules intended to restrict access to sensitive files and directories and prevent execution of selected binaries.

### 4. Install the Docker SDK for Python

```bash
pip install docker
```

### 5. Apply the AppArmor Option Using Python

```bash
python apply_apparmor.py
```

The script builds the Docker image, starts a container, and passes the following security option:

```text
apparmor=my-apparmor-profile
```

### 6. Verify the Container Configuration

Check the containers:

```bash
docker ps -a
```

Inspect the container security option:

```powershell
docker inspect 9341ac675780 --format '{{json .HostConfig.SecurityOpt}}'
```

Observed output:

```text
["apparmor=my-apparmor-profile"]
```

The output confirms that the AppArmor option was included in the Docker container configuration.

### 7. Check Container Logs

```bash
docker logs 9341ac675780
```

The Flask application started and listened on port 5000 inside the container.

## Application URL

When the container is running with port 5000 published, access the Flask application at:

http://localhost:5000

Note: The container used during verification has exited, so the URL will not respond until the application is running again.

## Environment Note

The lab was performed using Windows, Docker Desktop, and WSL2. The WSL2 environment reported that the AppArmor module was not enabled (`/sys/module/apparmor/parameters/enabled` returned `N`).

Therefore, although the Docker configuration contained the requested AppArmor option, actual kernel enforcement of the profile could not be confirmed in this environment.

## Result

Created the AppArmor profile file, built the Flask Docker image, installed the Docker SDK for Python, and executed the Python script to configure and inspect the container's AppArmor security option. Actual AppArmor enforcement remains unverified because of the WSL2 kernel limitation.

## Reference

Official exercise: [Docker Security with AppArmor](https://github.com/SunagP/DevOps-Lab/blob/main/Exercises/5-Docker-Security-AppArmor.md)

## Author

DevOps Lab 5 – Docker Security with AppArmor
