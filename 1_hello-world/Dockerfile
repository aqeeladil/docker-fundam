# Use the official Ubuntu image as the base image
FROM ubuntu:latest

# Set the working directory in the image
WORKDIR /app

# Copy the application files to the image
COPY . /app

# Install the necessary packages and clean up to reduce image size
RUN apt-get update && apt-get install -y python3 python3-pip && rm -rf /var/lib/apt/lists/*

# Install Python dependencies if a requirements.txt file exists
COPY requirements.txt /app/
RUN pip3 install --no-cache-dir -r requirements.txt

# Set environment variables
ENV NAME World

# Expose a port (if your app runs on a specific port, e.g., 5000 for Flask)
EXPOSE 5000

# Run a command to start the application
CMD ["python3", "app.py"]
