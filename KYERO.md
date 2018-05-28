# Kyero's Rendertron

This is a fork of [Google's Rendertron repository](https://github.com/GoogleChrome/rendertron).

We need our own fork in order to configure it the way we want and to push it to Heroku, since Google does not publish a pre-built image that would be configurable by environment variables and based on which we could build our own image easily 🙄

## Deploy to Heroku

Deploying to Heroku can be done easily thanks to their [container registry](https://devcenter.heroku.com/articles/container-registry-and-runtime):

0. If you're not logged with heroku container registry, just do it with:

   ```shell
   heroku container:login
   ```

1. Build the image:

    ```shell
    git fetch origin master && \
    git reset origin/master --hard && \
    docker build . --no-cache -t registry.heroku.com/kyero-staging-rendertron/web
    ```

2. Push it to Heroku (adapt for staging/production):

    ```shell
    docker push registry.heroku.com/kyero-staging-rendertron/web
    ```

3. Push it Heroku Container thing

   ```shell
   heroku container:push web --app kyero-staging-rendertron
   ```

> **NOTE**: the nginx buildback / rendertron doesn't deal too well with credentials, so you might need to remove HTTP basic auth credentials if they are in place (like @ staging).

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
