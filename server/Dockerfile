# # 1. Add a base image
# FROM python:3.12.0-slim-bookworm

# # 2. Environment settings (avoid .pyc files, ensure logs are unbuffered)
# ENV PYTHONDONTWRITEBYTECODE=1
# ENV PYTHONUNBUFFERED=1

# # 3. (Optional but common) Install OS-level deps, then clean up
# RUN apt-get update && apt-get install -y --no-install-recommends \
#     build-essential \
#  && rm -rf /var/lib/apt/lists/*

# # 4. Change the working directory
# WORKDIR /app

# # 5. Add the requirements.txt file
# COPY requirements.txt /app/

# # 6. Install and update Python (pip + dependencies)
# RUN pip install --upgrade pip \
#  && pip install -r requirements.txt

# # 7. Copy the rest of the application code
# COPY . /app/

# # 8. Expose the port your app listens on
# EXPOSE 8000

# # 9. Run the command to start the application
# # For a Django + Gunicorn app:
# CMD ["gunicorn", "--bind", "0.0.0.0:8000", "--workers", "3", "djangoproj.wsgi:application"]


FROM python:3.12.0-slim-bookworm

ENV PYTHONBUFFERED=1
ENV PYTHONWRITEBYTECODE=1

ENV APP=/app

# Change the workdir.
WORKDIR /app

# Install the requirements
COPY requirements.txt /app/

RUN pip3 install -r requirements.txt

# Copy the rest of the files
COPY . /app

RUN python manage.py collectstatic --noinput

EXPOSE 8000

RUN chmod +x /app/entrypoint.sh

ENTRYPOINT ["/bin/bash","/app/entrypoint.sh"]

CMD ["gunicorn", "--bind", ":8000", "--workers", "3", "djangoproj.wsgi"]