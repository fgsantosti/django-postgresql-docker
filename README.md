# django-postgresql-docker

# Django com PostgreSQL e Docker 

## Instalar o Django e Configurar um Ambiente Virtual 

### Crie um diretório
mkdir django-apps
cd django-apps

### Criando seu ambiente virtual. Vamos chamá-lo de generic env
virtualenv env

### Ative o ambiente virtual 
. env/bin/activate

### Pode instalar as bibliotecas necessárias, como exemplo:
pip install django

## Criando o projeto Django
django-admin startproject auladocker .

## Crie o arquivo requirements.txt e coloque as dependencias necessárias para o projeto
Django==3.1.2
psycopg2-binary==2.8.6

## Crie o arquivo Dockerfile e coloque as configurações necessárias 

FROM python:3.8

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

WORKDIR /code

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

## Criando o arquivo docker-compose.yml 

version: "3"

services:
  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - 8000:8000
    depends_on:
      - db
  db:
    image: postgres:13
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
    volumes:
      - postgres_data:/var/lib/postgresql/data/

volumes:
  postgres_data:

## Faça a configuração do arquivo settings do projeto inserindo a referencia do banco de dados utilizando

DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql",
        "NAME": "postgres",
        "USER": "postgres",
        "PASSWORD": "postgres",
        "HOST": "db",
        "PORT": 5432,
    }
}

## Executando os comandos para execução do docker-compose

docker-compose up 

### Comando para parar a aplicação 

docker-compose down 

### Comando para subir a aplicação depois de modificações na app
docker-compose up --build

docker-compose up -b

### Para verificar os logs quando estiver em backlog 

docker-compose logs

### Para executar os comantos diretamente no contaner 

docker-compose exec 

### Executando os comandos em uma aplicação django 
docker-compose exec web python manage.py migrate 

web- é o nome da app que foi dados no 

## Criando super usuário na app
docker-compose exec web python manage.py createsuperuser 

## Para verificar se os dados estão sendo persistidos no banco de dados  
docker-compose down 
docker-compose up 
ou
docker-compose up -d 

### Qualquer dúvida acione 
docker-compose --help
w
