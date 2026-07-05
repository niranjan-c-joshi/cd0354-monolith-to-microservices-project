## Build
FROM --platform=$BUILDPLATFORM node:20-bullseye AS ionic

RUN npm install -g @ionic/cli

# Create app directory
WORKDIR /usr/src/app
# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
COPY package*.json ./
RUN npm install --legacy-peer-deps
# Bundle app source
COPY . .

ENV NODE_OPTIONS=--max-old-space-size=6144

RUN ionic build || ( \
  echo "==== Angular errors log(s) ====" && \
  for f in /tmp/ng-*/angular-errors.log; do \
    echo "---- $f ----"; \
    cat "$f"; \
  done && \
  exit 1 \
)

## Run 
FROM nginx:alpine

COPY --from=ionic /usr/src/app/www /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf