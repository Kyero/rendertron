# Kyero's Rendertron

This is a fork of [Google's Rendertron repository](https://github.com/GoogleChrome/rendertron).

We need our own fork in order to configure it the way we want and to push it to Heroku, since Google does not publish a pre-built image that would be configurable by environment variables and based on which we could build our own image easily 🙄

## Deploy to Heroku

Deploying to Heroku can be done easily thanks to their [container registry](https://devcenter.heroku.com/articles/container-registry-and-runtime):

1. Build the image:

    ```shell
    git fetch origin master && \
    git reset origin/master --hard && \
    docker build . --no-cache -t registry.heroku.com/kyero-staging-rendertron/web -t registry.heroku.com/kyero-production-rendertron/web
    ```

2. Push it to Heroku (adapt for staging/production):

    ```shell
    docker push registry.heroku.com/kyero-staging-rendertron/web
    ```

> 💡 We could eventually push this image as a dyno of `frontend` (rather to its own app) but we would need a way to forward HTTP requests from one dyno (`web`) to another (`rendertron`), which I don't think is possible at the moment.

## Rebase from upstream

You need the [upstream repository](https://github.com/GoogleChrome/rendertron) configured as a git remote:

```shell
git remote add upstream git@github.com:GoogleChrome/rendertron.git
```

Then you can pull latest developments, rebase our modifications and push that to our own repository:

```shell
git fetch upstream master  && \
git rebase upstream/master && \
git push --force-with-lease origin master
```
