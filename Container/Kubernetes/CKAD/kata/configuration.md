# Configurations
From: https://github.com/bmuschko/ckad-crash-course/tree/master/exercises

## Configuring a Pod to Use a ConfigMap
1. Create a new file named `config.txt` with the following environment variables as key/value pairs on each line.

- `DB_URL` equates to `localhost:3306`
- `DB_USERNAME` equates to `postgres`

2. Create a new ConfigMap named `db-config` from that file.
3. Create a Pod named `backend` that uses the environment variables from the ConfigMap and runs the container with the image `nginx`.
4. Shell into the Pod and print out the created environment variables. You should find `DB_URL` and `DB_USERNAME` with their appropriate values.
5. (Optional) Discuss: How would you approach hot reloading of values defined by a ConfigMap consumed by an application running in Pod?

## Configuring a Pod to Use a Secret

1. Create a new Secret named `db-credentials` with the key/value pair `db-password=passwd`.
2. Create a Pod named `backend` that uses the Secret as environment variable named `DB_PASSWORD` and runs the container with the image `nginx`.
3. Shell into the Pod and print out the created environment variables. You should find `DB_PASSWORD` variable.
4. (Optional) Discuss: What is one of the benefit of using a Secret over a ConfigMap?

## Creating a Security Context for a Pod

1. Create a Pod named `secured` that uses the image `nginx` for a single container. Mount an `emptyDir` volume to the directory `/data/app`.
2. Files created on the volume should use the filesystem group ID 3000.
3. Get a shell to the running container and create a new file named `logs.txt` in the directory `/data/app`. List the contents of the directory and write them down.

## Defining a Pod’s Resource Requirements

Create a resource quota named `app` under the namespace `rq-demo` using the following YAML definition in the file `rq.yaml`.

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: app
spec:
  hard:
    pods: "2"
    requests.cpu: "2"
    requests.memory: 500m
```

1. Create a new Pod that exceeds the limits of the resource quota requirements e.g. by defining 1G of memory. Write down the error message.
2. Change the request limits to fulfill the requirements to ensure that the Pod could be created successfully. Write down the output of the command that renders the used amount of resources for the namespace.

## Using a ServiceAccount

1. Create a new service account named `backend-team`.
2. Print out the token for the service account in YAML format.
3. Create a Pod named `backend` that uses the image `nginx` and the identity `backend-team` for running processes.
4. Get a shell to the running container and print out the token of the service account.
