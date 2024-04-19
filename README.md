# Car Dealership Web application 

## Overview
This project is the culmination of the IBM Full Stack Developer Professional Certificate, serving as the Capstone project. The aim is to develop a web portal for a national car dealership, allowing customers to access dealership reviews across the country. The website facilitates viewing dealership information, filtering by state, reading reviews, and adding new reviews. It utilizes Django for the backend, React for the frontend, MongoDB for the database, and Docker for containerization.

## Project Breakdown

1. **Create Static Pages:**
    - Developed static pages to fulfill user stories.

2. **User Management:**
    - Implemented user authentication using Django's user authentication system.
    - Created a REACT frontend for user interaction.

5. **Backend Services:**
    - Developed a Node.js server to manage dealers and reviews using MongoDB.
    - Dockerized the service for easy deployment.

6. **Sentiment Analyzer Service:**
    - Deployed a sentiment analyzer on IBM Cloud Code Engine to analyze review sentiments.

7. **Dynamic Pages:**
    - Created dynamic pages using Django templates to display dealers and reviews.

8. **CI/CD and Testing:**
    - Set up continuous integration and delivery for code linting.
    - Tested the application locally and deployed on Kubernetes.

## Solution Architecture
The project utilizes multiple technologies:
- Django for the main web application.
- SQLite for storing Car Make and Car Model data.
- Express MongoDB service running in a Docker container for managing dealerships and reviews.
- IBM Cloud Code Engine hosts the Sentiment Analyzer Service.

## User Roles and Use Cases
### Anonymous Users:
- View Contact Us and About Us pages.
- Browse dealerships and filter them by state.
- View reviews for selected dealerships.

### Authorized Users:
- Write reviews for dealerships.
- Reviews include details like user information, review text, purchase details, etc.

### Admin Users:
- Log in to the admin site.
- Add new make, model, and other attributes.

## Deployment
The application has been containerized using Docker for easy deployment. It has been deployed on IBM Cloud Kubernetes Service for scalability and reliability.
