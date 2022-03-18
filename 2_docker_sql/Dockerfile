# What image
FROM python:3.9
# What to run while installing
RUN pip install pandas
# Name of working directory in the image
WORKDIR /app
# Copy source to destination
COPY pipeline.py pipeline.py
# Starts with bash
#ENTRYPOINT ["bash"]
# Just runs the file
ENTRYPOINT ["python", "pipeline.py"]