FROM  python:3.8-buster
LABEL maintainer="Mariem Ben salah"
COPY  requirements.txt student_age.py  /
# Utiliser les dépôts archivés pour Buster
RUN sed -i 's|http://deb.debian.org/debian|http://archive.debian.org/debian|g' /etc/apt/sources.list && \
    apt-get update -y && \
    apt-get install -y python3-dev libsasl2-dev libldap2-dev libssl-dev build-essential && \
    pip install --no-cache-dir -r /requirements.txt
VOLUME [ "/data" ]
EXPOSE 5000
CMD [ "python3", "./student_age.py" ]