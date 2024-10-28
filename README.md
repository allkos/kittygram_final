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

Clone the repository and navigate to it in the command line:

```bash
git clone git@github.com/allkos/kittygram_final
```
Create and activate a virtual environment:
```
python3.9 -m venv venv
```
If you are using Linux/macOS:
```
source venv/bin/activate
```
If you are using Windows:
```
source venv/scripts/activate
```
Upgrade pip:
```
python3.9 -m pip install --upgrade pip
```
Install dependencies from requirements.txt:
```
pip install -r requirements.txt
```
Set the database credentials in the .env file, using .env.example as a reference:
```
POSTGRES_USER=username
POSTGRES_PASSWORD=password
POSTGRES_DB=database_name
DB_HOST=host_name
DB_PORT=5432
SECRET_KEY=django_settings_secret_key
ALLOWED_HOSTS=127.0.0.1, localhost
```
Start Docker Compose with the default configuration (docker-compose.yml):

Build the containers:
```
sudo docker compose up -d --build
```
Apply migrations:
```
sudo docker compose exec backend python manage.py migrate
```
Create a superuser:
```
sudo docker compose exec backend python manage.py createsuperuser
```
Collect static files:
```
sudo docker compose exec backend python manage.py collectstatic
```
Copy static files to /backend_static/static/ in the backend container:
```
sudo docker compose exec backend cp -r /app/collected_static/. /backend_static/static/
```
Go to 127.0.0.1:8000

Request Examples

Example GET request : View the list of achievements.
GET .../api/achievements/

Example response:
```
[
    {
        "id": 1,
        "achievement_name": "jump-jump"
    },
    {
        "id": 2,
        "achievement_name": "fights with reflection in the mirror"
    }
]
```

Example POST request: Add a new cat.
POST .../api/cats/

Request body:
```
{
    "name": "Judith",
    "color": "#FF8C00",
    "birth_year": 1987
}
```
Example response:
```
{
    "id": 7,
    "name": "Judith",
    "color": "darkorange",
    "birth_year": 1987,
    "achievements": [],
    "owner": 2,
    "age": 36,
    "image": null
}
```
