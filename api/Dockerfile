#Use an official Python runtime as the base image
FROM python:3.9

#Set the working directory in the container
WORKDIR /app

#Copy the requirements file to the working directory
COPY requirements.txt .

# Install the required packages
RUN pip install --no-cache-dir -r requirements.txt

#Copy the rest of the application code
COPY . .

#Expose the port that FastAPI runs on
EXPOSE 8000

#Start the FastAPI application
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]