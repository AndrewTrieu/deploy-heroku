## A deployment pipeline to Heroku
This repository was created for Exercise 3.1 of the MOOC course DevOps with Docker (3 ECTS) of the University of Helsinki. The course page was cloned and published to Heroku.
Link: https://devops-coursepage-helsinki.herokuapp.com/
## Deployment
.github/workflows/build.yml
```yml
name: Build and Deploy

on:
  push:
    branches:
      - master
jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - name: Login to DockerHub
        uses: docker/login-action@v1
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build and push
        uses: docker/build-push-action@v2
        with:
          push: true
          tags: ${{ secrets.DOCKERHUB_USERNAME }}/devopspage:latest
      - name: Deploy on heroku
        uses: akhileshns/heroku-deploy@v3.12.12
        with:
          heroku_api_key: ${{ secrets.HEROKU_API_KEY }}
          heroku_app_name: ${{ secrets.HEROKU_APP_NAME }}
          heroku_email: ${{ secrets.HEROKU_EMAIL }}
          usedocker: true
```
