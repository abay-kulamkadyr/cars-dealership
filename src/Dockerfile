# --- STAGE 1: Build the frontend assets using a Node.js image ---
FROM node:20.12.2-slim AS node_builder

# Set the working directory for the frontend build
WORKDIR /frontend

# Copy the frontend application code
COPY ./frontend .

# Install dependencies and build the frontend for production
RUN npm install
RUN npm run build

# --- STAGE 2: Build the final Python image for the Django app ---
FROM python:3.12.0-slim-bookworm

# Set environment variables
ENV PYTHONBUFFERED 1
ENV PYTHONWRITEBYTECODE 1
ENV APP=/app

# Change the workdir inside the container
WORKDIR $APP

# Install the requirements
# We copy the requirements file first to take advantage of Docker's cache
COPY requirements.txt $APP/
RUN pip3 install -r $APP/requirements.txt

# Copy the built frontend assets from the node_builder stage
# This assumes the build output is in a 'build' directory inside the frontend folder
COPY --from=node_builder /frontend $APP/frontend

# Explicitly copy the entrypoint script
# This ensures it's part of the image and not reliant on the volume mount
COPY entrypoint.sh $APP/

# Make the entrypoint script executable
RUN chmod +x $APP/entrypoint.sh

# Copy all other necessary application files
# We replace COPY . $APP with these specific lines to avoid copying
# the database and sentiment-analyzer directories.
COPY djangoapp $APP/djangoapp
COPY djangoproj $APP/djangoproj
COPY manage.py $APP/manage.py
COPY scripts.sh $APP/scripts.sh

# Expose the port your application listens on
EXPOSE 8000

# Set the entrypoint to run the script.
# The `entrypoint.sh` file already has a shebang (`#!/bin/sh`),
# so we can directly execute the script without explicitly calling /bin/bash.
ENTRYPOINT ["/app/entrypoint.sh"]

# Set the default command to run gunicorn, which will be executed
# by the entrypoint script.
CMD ["gunicorn", "--bind", ":8000", "--workers", "3", "djangoproj.wsgi"]
