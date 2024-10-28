![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white) ![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E) ![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54) ![Django](https://img.shields.io/badge/django-%23092E20.svg?style=for-the-badge&logo=django&logoColor=white) ![DjangoREST](https://img.shields.io/badge/DJANGO-REST-ff1709?style=for-the-badge&logo=django&logoColor=white&color=ff1709&labelColor=gray) ![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white) ![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white) ![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white) ![GitHub Actions](https://img.shields.io/badge/github%20actions-%232671E5.svg?style=for-the-badge&logo=githubactions&logoColor=white)

## Project Features

The project allows users to register, log in, add new cats to the site or update existing ones, add or edit achievements, and view other users' posts.

The API for Kittygram is built using Django REST Framework, with TokenAuthentication for authentication, and the Djoser library for additional functionalities.

The project is deployed in multiple Docker containers. The backend container is configured with the Gunicorn WSGI server. Project testing and deployment automation for Kittygram is managed with GitHub Actions.

## CI/CD Configuration

Deployment to the remote server is configured through CI/CD using GitHub Actions. The workflow includes:

- Backend code is checked for PEP8 compliance;
- Automated testing is set up for both frontend and backend;
- Upon successful tests, images are updated on Docker Hub;
- Images on the server are updated, and the application restarts with Docker Compose;
- Commands are executed to collect static files for the backend, move static files to a volume, and apply migrations;
- A notification is sent to Telegram upon successful deployment.

An example of this workflow is saved in the kittygram_workflow.yml file located in the root directory of the project. To run this workflow, copy its contents to .github/workflows/main.yml.

Save the following variables with the necessary values in GitHub Actions secrets:

- DOCKER_USERNAME - Docker Hub username
- DOCKER_PASSWORD - Docker Hub password
- SSH_KEY - Private SSH key for accessing the production server
- SSH_PASSPHRASE - Passphrase for this SSH key
- USER - Username for accessing the production server
- HOST - Host address for accessing the production server
- TELEGRAM_TO - Telegram account ID
- TELEGRAM_TOKEN - Token for the Telegram bot

The docker-compose.yml file for local project deployment and the docker-compose.production.yml file for deployment on a cloud server are located in the root directory of the project.

## Stack

- Python 3.9
- Django 3.2.3
- Django REST framework 3.12.4
- Nginx
- Docker Compose

## Running the Project in Development Mode

Create a project directory named `kittygram` and navigate into it:

```bash
mkdir kittygram
cd kittygram

Copy (or create) the docker-compose.production.yml file in the project directory and start the project:

sudo docker compose -f docker-compose.production.yml up

This will download the images, create and start containers, and set up volumes and networking.

Launching the Project from GitHub Source Code

Clone the repository:

git clone git@github.com/allkos/kittygram_final

Start the project:

sudo docker compose -f docker-compose.yml up

After Launch: Migrations and Static Files

After launching, run migrations and collect static files for the backend. Frontend static files are collected during container startup, and then the container stops.

sudo docker compose -f [docker-compose-file-name.yml] exec backend python manage.py migrate
sudo docker compose -f [docker-compose-file-name.yml] exec backend python manage.py collectstatic
sudo docker compose -f [docker-compose-file-name.yml] exec backend cp -r /app/collected_static/. /static/static/

The project will now be available at:

http://localhost:9000/

Environment Variables

Below is an example of a .env file with environment variables needed to run the application:

POSTGRES_DB=kittygram
POSTGRES_USER=kittygram_user
POSTGRES_PASSWORD=kittygram_password
DB_NAME=kittygram
DB_HOST=db
DEBUG=False
SECRET_KEY=django_secret_key_example

Stopping the Containers

Press Ctrl+C in the window where the project is running, or run the following command in another terminal window:

sudo docker compose -f docker-compose.yml down

