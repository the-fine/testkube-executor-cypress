# syntax=docker/dockerfile:1
FROM golang:1.18
WORKDIR /build
COPY . .
ENV CGO_ENABLED=0 
ENV GOOS=linux

RUN cd cmd/agent;go build -o /runner -mod mod -a .

# using debian based image as alpine is not supported by Cypress yet
# https://github.com/cypress-io/cypress/issues/419 

FROM node:17
RUN apt-get update && apt-get install -y libgtk2.0-0 \
    libgtk-3-0 \
    libgbm-dev \
    libnotify-dev \
    libgconf-2-4 \
    libnss3 \
    libxss1 \
    libasound2 \
    libxtst6 \
    xauth \
    xvfb \
	git
 
RUN npm install -g cypress browserslist@latest

# node:17 is already adding userid=1000 for nodejs! 
# so we can use it inside our docker container.
 
RUN mkdir /home/runner
RUN chown -R 1000:1000 /home/runner

WORKDIR /home/runner/

COPY --from=0 --chown=1000:1000 /runner /bin/runner

USER 1000

ENTRYPOINT ["/bin/runner"]
