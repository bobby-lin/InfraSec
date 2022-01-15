# Pod Design

Links:
https://github.com/bmuschko/ckad-crash-course/tree/master/exercises/11-using-labels-annotations
https://github.com/bmuschko/ckad-crash-course/tree/master/exercises/12-rolling-updates-deployment
https://github.com/bmuschko/ckad-crash-course/tree/master/exercises/13-creating-cronjob

## Defining and Querying Labels and Annotations

1. Create three different Pods with the names `frontend`, `backend` and `database` that use the image `nginx`.
2. Declare labels for those Pods as follows:

- `frontend`: `env=prod`, `team=shiny`
- `backend`: `env=prod`, `team=legacy`, `app=v1.2.4`
- `database`: `env=prod`, `team=storage`

3. Declare annotations for those Pods as follows:

- `frontend`: `contact=John Doe`, `commit=2d3mg3`
- `backend`: `contact=Mary Harris`

4. Render the list of all Pods and their labels.
5. Use label selectors on the command line to query for all production Pods that belong to the teams `shiny` and `legacy`.
6. Remove the label `env` from the `backend` Pod and rerun the selection.
7. Render the surrounding 3 lines of YAML of all Pods that have annotations.

## Performing Rolling Updates for a Deployment

1. Create a Deployment named `deploy` with 3 replicas. The Pods should use the `nginx` image and the name `nginx`. The Deployment uses the label `tier=backend`. The Pods should use the label `app=v1`.
2. List the Deployment and ensure that the correct number of replicas is running.
3. Update the image to `nginx:latest`.
4. Verify that the change has been rolled out to all replicas.
5. Scale the Deployment to 5 replicas.
6. Have a look at the Deployment rollout history.
7. Revert the Deployment to revision 1.
8. Ensure that the Pods use the image `nginx`.
9. (Optional) Discuss: Can you foresee potential issues with a rolling deployment? How do you configure a update process that first kills all existing containers with the current version before it starts containers with the new version?

## Creating a Scheduled Container Operation

1. Create a CronJob named `current-date` that runs every minute and executes the shell command `echo "Current date: $(date)"`.
2. Watch the jobs as they are being scheduled.
3. Identify one of the Pods that ran the CronJob and render the logs.
4. Determine the number of successful executions the CronJob will keep in its history.
5. Delete the Job.
