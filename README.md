# Voting app Using Redis and Flask

A simple voting app that uses Redis for a data store and a Python Flask app for the frontend.

- 
- Builds the Docker image
- Provisions AWS Container Registry (ECR) instance
- Pushes the image to the ECR instance
- Creates a new ECS task definition, pointing to the ECR image definition

## Prerequisites

To use this example, make sure [Docker](https://docs.docker.com/engine/installation/) is installed and running.

## Deploying and running the program

### Configure the deployment

Note: some values in this example will be different from run to run.  These values are indicated
with `***`.

1.  Login via `pulumi login`.

1.  Create a new stack:

    ```
    $ pulumi stack init voting-app-testing
    ```

1.  Set AWS as the provider:

    ```
    $ pulumi config set cloud:provider aws
    ```

1.  Configure Pulumi to use an AWS region that supports Fargate, which is currently only available in `us-east-1`, `us-east-2`, `us-west-2`, and `eu-west-1`:

    ```
    $ pulumi config set aws:region us-west-2
    ```

1.  Set a value for the Redis password. The value can be an encrypted secret, specified with the `--secret` flag. If this flag is not provided, the value will be saved as plaintext in `Pulumi.testing.yaml` (since `testing` is the current stack name).

    ```
    $ pulumi config set --secret redisPassword S3cr37Password
    ```

### Install dependencies

1.  Restore NPM modules via `npm install` or `yarn install`.

### Preview and deploy

1.  Ensure the Docker daemon is running on your machine, then preview and deploy the program with `pulumi up`. The program deploys 24 resources and takes about 10 minutes to complete.

1.  View the stack output properties via `pulumi stack output`. The stack output property `frontendUrl` is the URL and port of the deployed app:

    ```bash
    $ pulumi stack output frontendURL
    ***.elb.us-west-2.amazonaws.com
    ```

1.  In a browser, navigate to the URL for `frontendURL`. You should see the voting app webpage.

   ![Voting app screenshot](./voting-app-webpage.png)

### Delete resources

When you're done, run `pulumi destroy` to delete the program's resources:

```
$ pulumi destroy
This will permanently destroy all resources in the 'testing' stack!
Please confirm that this is what you'd like to do by typing ("testing"): testing
```

## About the Project 

The project  is meant to cement what has been taught in the module

You should create a build server to manage application and infrastructure pipeline. Jenkins should be used for the pipeline.
Use Continous Integration mechanism as much as possible
Set up Prometheus to manage infrastructure and application monitoring and alerting 
All application should run on Docker 
Use Amazon  EC2 as node
 


## About the App

The application is a voting app , written in Python Flask.

### Resources
The definition of `frontend` is more interesting, as it uses `image` property of `FargateService.taskDefinitionArgs` to point to a folder with a Dockerfile, which in this case is a Python Flask app. Pulumi automatically invokes `docker build` for you and pushes the container to ECR.

So that the `frontend` container can connect to `redisCache`, the environment variables `REDIS`, `REDIS_PORT` are defined. Using the `redisListenre.endpoint` property, it's easy to declare the connection between the two containers.

The Flask app uses these environment variables to connect to the Redis cache container. See the following in [`frontend/app/main.py`](frontend/app/main.py):

```python
redis_server =   os.environ['REDIS']
redis_port =     os.environ['REDIS_PORT']
redis_password = os.environ['REDIS_PWD']
```
