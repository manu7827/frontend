# Use an official lightweight Python image.
#
https://hub.docker.com/_/python
FROM python:3.8-slim
# Set the working directory to /app
WORKDIR /app
# Copy the current directory contents into the container at /app
COPY . /app
# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir flask
# Define environment variable
ENV STATIC_URL /static
ENV STATIC_PATH /app/static
# Write a simple Flask app to serve the HTML file
RUN echo "from flask import Flask, send_from_directory\n\
app = Flask(__name__, static_url_path=STATIC_URL)\n\
@app.route('/')\n\
def serve_index():\n\
   return send_from_directory(STATIC_PATH, 'index.html')\n\
if __name__ == '__main__':\n\
   app.run(host='0.0.0.0', port=80)" > app.py
# Make port 80 available to the world outside this container
EXPOSE 80
# Define environment variable
ENV NAME World
# Run app.py when the container launches
CMD ["python", "app.py"]