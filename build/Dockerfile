FROM nikolaik/python-nodejs:python3.11-nodejs18-bullseye
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -

# Install nodejs cli utils
RUN npm install -g nodemon concurrently

# Install ufbt
RUN apt update && apt install git cron -y
RUN git clone https://github.com/flipperdevices/flipperzero-ufbt /opt/ufbt
# RUN python /opt/ufbt/ufbt/bootstrap.py update
RUN python -m pip install --upgrade /opt/ufbt
# RUN /opt/ufbt/ufbt update
# python3 -m pip install --upgrade ufbt


# Cron task to search apps
RUN crontab -l | { cat; echo "*/2 * * * * python /app/backend/search.py"; } | crontab -

# Install backend
ADD backend /app/backend
WORKDIR /app/backend
RUN pip install -r requirements.txt

# Install frontend
ADD frontend /app/frontend
WORKDIR /app/frontend
RUN NODE_ENV=development npm i

WORKDIR /app
ENTRYPOINT [ "concurrently", "cd /app/backend && flask db upgrade && nodemon app.py", "cd /app/frontend && npm run watch", "cron" ]