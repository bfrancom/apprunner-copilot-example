# AWS App Runner using AWS Copilot CLI Demo

## Create our application

```bash
copilot app init apprunner-demo
```

> **NOTE:** If you have a domain to have a vanity url for this demo, run the following:

```bash
copilot app init --domain domainname.com
```

## Create our environment

```bash
copilot env init --name test --profile default --default-config
```

## Create our service

```bash
copilot svc init --name user-api --svc-type "Request-Driven Web Service" --dockerfile ./Dockerfile
```

## Create our NoSQL Database table

```bash
copilot storage init -n users -t DynamoDB -w user-api --partition-key first_name:S --sort-key last_name:S --no-lsi
```

## Deploy our environment

```bash
copilot svc deploy
```

## Testing the application

#### Load the database

First, grab the url for your service.

```bash
lb_url=$(copilot svc show --name user-api --json | jq -r '.routes[0].url')
```

Run the following command to load the database:

```bash
curl -XPOST -s $lb_url/load_db
```

Now you can query the application by running the following commands:

#### Query specific user

```bash
curl -s "$lb_url/user/?first=Sheldon&last=Cooper"
```

#### Query all users

```bash
curl -s $lb_url/all_users
```
