---
title: "Vue.js in Docker"
date: 2019-08-13T11:07:26-07:00
draft: false
---

This post will show how to create a simple [Vue.js](https://vuejs.org/) single page application and host it in a docker container.

## Create the application

Install the Vue command line interface (cli) [from here](https://cli.vuejs.org/guide/#cli). I'll be using yarn for this example..

{{< highlight bash >}}
yarn global add @vue/cli
{{< /highlight >}}

Create a new hello-world application and change into the application's folder.

{{< highlight bash >}}
# this places the new application in a ./hello-world folder
vue create hello-world
cd hello-world
yarn install
{{< /highlight >}}

## Build a Docker image

Now that we have a working Vue.js application, we can put it in a container and serve it up using [nginx](https://www.nginx.com/).

We'll use a [multi-stage](https://docs.docker.com/develop/develop-images/multistage-build/) docker build to first build the application and the serve it.

Create a file called "Dockerfile" in the root directory of your project. It should look like this:

{{< highlight text >}}
FROM node
WORKDIR /app
COPY package.json ./
COPY yarn.lock ./
RUN yarn install
COPY . .
RUN yarn build

FROM nginx
COPY --from=0 /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
{{< /highlight >}}

After creating the Dockerfile, build the image.

{{< highlight bash >}}
docker build -t hello-world .
{{< /highlight >}}

Now you can run a container with you application. Notice the exposed port 8080 that allows your application to be reached on that port.

{{< highlight bash >}}
docker run -d -p 8080:80 --name hello-world hello-world
{{< /highlight >}}

You should be able to access your application at http://localhost:8080

## Reference

[Dockerize Vue.js App](https://vuejs.org/v2/cookbook/dockerize-vuejs-app.html) 


