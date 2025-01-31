# Cars Dealership Web Application

A containerized full-stack solution managing car dealerships, user reviews, and sentiment analysis. The stack includes **Django** for core backend logic, **Node.js/Mongoose** for dealer/review data in MongoDB, **React** for a user-friendly SPA, and **Flask** for sentiment analysis—**hosted on IBM Cloud Code Engine**.

## Table of Contents
- [Overview](#overview)
- [Architecture](#architecture)
- [Key Features](#key-features)
- [Technologies](#technologies)
- [Project Structure](#project-structure)
- [Setup & Usage](#setup--usage)
- [Docker Deployments](#docker-deployments)
- [License](#license)

---

## Overview
- **Multi-Service Architecture**:
  1. **Django**: Orchestrates user login, registration, and partial data handling.
  2. **Node.js** (Express + Mongoose): Stores dealerships, reviews in MongoDB.
  3. **React**: Provides the front-end Single-Page Application (SPA).
  4. **Flask (Sentiment Service)**: Deployed on **IBM Cloud Code Engine**, performing NLTK-based sentiment analysis on posted reviews.

- **Interactions**:
  - The Node/Mongo service is launched via `docker-compose.yml`.
  - Django communicates with Node’s endpoints to fetch or insert data.
  - React calls Django routes, which in turn query Node or pass text to the Flask microservice for sentiment scoring.

---

## Architecture
![Architecture](./assets/project_architecture.png)
1. **Django** (`djangoproj` / `djangoapp`):
   - Provides endpoints for login (`/djangoapp/login`), registration, fetching local car info, and bridging requests to Node-based dealership data.
   - Uses SQLite for its local models (CarMake, CarModel).

2. **Node.js + MongoDB** (`database` folder):
   - Node server (`app.js`) with Mongoose, connecting to a MongoDB container.
   - Exposes routes like `/fetchDealers`, `/fetchReviews`, `/insert_review`.

3. **React** (`frontend` folder):
   - SPA with routes `/login`, `/register`, `/dealers`, `/dealer/:id`, and more.
   - Communicates with Django using fetch/REST calls.

4. **Flask Sentiment** (in `djangoapp/microservices`):
   - **Hosted on IBM Cloud Code Engine** for serverless or container-based deployment.
   - Provides an `/analyze/<input_txt>` route using NLTK’s VADER for sentiment detection.

---

## Key Features
- **Dealership & Reviews**: Node.js microservice supplies dealership info, seeded by JSON. Reviews are stored and retrieved with Mongoose.
- **Sentiment Analysis**: The Flask service on IBM Cloud Code Engine returns “positive,” “negative,” or “neutral” for each review.
- **Authentication**: Django handles user login/register. Session-based logic ensures only authenticated users post new reviews.
- **Docker**: Each service (Django, Node, MongoDB) containerized. `docker-compose.yml` spins up the Node + Mongo environment.

---

## Technologies
- **Languages & Frameworks**:
  - Python 3.9+ (Django, Flask)
  - Node.js (Express + Mongoose)
  - React (Create React App)
- **Cloud**:
  - **IBM Cloud Code Engine** for hosting the Flask microservice
- **Databases**:
  - MongoDB (for dealership/review data)
  - SQLite (for Django’s local data)
- **DevOps**:
  - Docker, Docker Compose
  - Gunicorn for Django production server

---

## Setup & Usage

### 1. **Node + Mongo**  
- `cd database/`  
- `docker-compose up -d` (starts MongoDB + Node server at `:3030`)

### 2. **Django**  
- Install dependencies: `pip install -r requirements.txt`  
- `python manage.py migrate`  
- `python manage.py runserver` (runs on `:8000` by default)  
*(Alternatively, build & run the Django Dockerfile.)*

### 3. **React**  
- `cd frontend/`  
- `npm install && npm start`  
- Access the SPA typically on `:3000`

### 4. **Flask**  
- Deployed to **IBM Cloud Code Engine**. Locally, you can run:
  ```bash
  cd djangoapp/microservices
  docker build -t flask-sentiment .
  docker run -p 5050:5050 flask-sentiment

- Update sentiment_analyzer_url in .env or Django’s settings if hosting externally.

## Docker Deployments
    Node + Mongo: docker-compose.yml in database/.
    Django: docker build -t djangoapp . && docker run -p 8000:8000 djangoapp.
    Flask: Deployed to IBM Cloud Code Engine. Locally, docker build ... & docker run ....
