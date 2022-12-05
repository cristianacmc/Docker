# Use Python 3.6 or later as a base image
FROM python:3.6
# Copy contents into image
WORKDIR /app
COPY app.py requirements.txt ./
# Install pip dependencies from requirements
RUN pip install -r requirements.txt
# Set YOUR_NAME environment variable
ENV YOUR_NAME="Cristiana"       
# Expose the correct port       
EXPOSE 5500
# create an entrypoint 
ENTRYPOINT ["python3", "app.py"]