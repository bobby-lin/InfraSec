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

```
k run frontend --image=nginx -l env=prod,team=shiny
k run backend --image=nginx -l env=prod,team=legacy,app=v1.2.4
k run database --image=nginx -l env=prod,team=storage
```

3. Declare annotations for those Pods as follows:

- `frontend`: `contact=John Doe`, `commit=2d3mg3`
- `backend`: `contact=Mary Harris`

```
k annotate pods frontend 'contact=John Doe',commit=2d3mg3
k annotate pods backend 'contact=Mary Harris'
```

4. Render the list of all Pods and their labels.
```
k get pods --show-labels
```
5. Use label selectors on the command line to query for all production Pods that belong to the teams `shiny` and `legacy`.
```
$ k get pods -l 'team in (shiny,legacy)'
NAME       READY   STATUS    RESTARTS   AGE
backend    1/1     Running   0          2m58s
frontend   1/1     Running   0          3m2s
```
6. Remove the label `env` from the `backend` Pod and rerun the selection.
```
k label pods backend env-
```
7. Render the surrounding 3 lines of YAML of all Pods that have annotations.

## Performing Rolling Updates for a Deployment

1. Create a Deployment named `deploy` with 3 replicas. The Pods should use the `nginx` image and the name `nginx`. The Deployment uses the label `tier=backend`. The Pods should use the label `app=v1`.
```
 kubectl create deployment deploy --replicas=3 --image=nginx --dry-run=client -o yaml
 # Then add labels
```
2. List the Deployment and ensure that the correct number of replicas is running.
```
$ k get deployments
NAME                READY   UP-TO-DATE   AVAILABLE   AGE
deploy              3/3     3            3           68s
my-deploy           1/1     1            1           2d12h
my-dp               3/3     3            3           2d13h
server-deployment   2/2     2            2           75m

$ kgp | grep deploy-
deploy-64f8876f95-crqtd              1/1     Running     0              91s
deploy-64f8876f95-jh6xq              1/1     Running     0              91s
deploy-64f8876f95-xwzln              1/1     Running     0              91s
my-deploy-79fbb965cd-cq9mj           1/1     Running     1 (177m ago)   2d12h
```
3. Update the image to `nginx:latest`.
```
kubectl set image deployment deploy nginx=nginx:latest # [container name]=[image name]:[image version]
```
4. Verify that the change has been rolled out to all replicas.
```
$ k describe pods deploy-8fcc787bb-2j2sg | grep image
  Normal  Pulling    106s  kubelet            Pulling image "nginx:latest"
  Normal  Pulled     103s  kubelet            Successfully pulled image "nginx:latest" in 2.9328884s
```
5. Scale the Deployment to 5 replicas.
```
$ k scale deployment deploy --replicas=5
deployment.apps/deploy scaled
```
6. Have a look at the Deployment rollout history.
```
$ k rollout history deployment deploy
deployment.apps/deploy 
REVISION  CHANGE-CAUSE
1         <none>
2         <none
```
7. Revert the Deployment to revision 1.
```
k rollout undo deployment deploy --to-revision=1
```
8. Ensure that the Pods use the image `nginx`.
```
$ k describe pods deploy-64f8876f95-sm7h5 | grep image
  Normal  Pulling    55s   kubelet            Pulling image "nginx"
  Normal  Pulled     52s   kubelet            Successfully pulled image "nginx" in 2.9156288s
```
9. (Optional) Discuss: Can you foresee potential issues with a rolling deployment? How do you configure a update process that first kills all existing containers with the current version before it starts containers with the new version?
```
Change the update strategy to "Recreate"
```

## Creating a Scheduled Container Operation

1. Create a CronJob named `current-date` that runs every minute and executes the shell command `echo "Current date: $(date)"`.
```
k create cronjob current-date --schedule="*/1 * * * *" --image=nginx -- //bin//sh -c 'echo "Current date: $(date)"'
```
2. Watch the jobs as they are being scheduled.
3. Identify one of the Pods that ran the CronJob and render the logs.
```
$ k logs current-date-27370413--1-4c9fs
Current date: Sat Jan 15 05:33:03 UTC 2022
```
4. Determine the number of successful executions the CronJob will keep in its history.
```
# Look at "Successful Job History Limit"
$ k describe cronjob/current-date
Name:                          current-date
Namespace:                     default
Labels:                        <none>
Annotations:                   <none>
Schedule:                      */1 * * * *
Concurrency Policy:            Allow
Suspend:                       False
Successful Job History Limit:  3
Failed Job History Limit:      1
[...]
```
5. Delete the Job.
```
$ k delete cronjob/current-date
cronjob.batch "current-date" deleted
```
