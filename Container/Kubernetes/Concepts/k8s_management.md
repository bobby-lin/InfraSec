# Kubernetes Management Techniques
There are different management techniques for K8s.
- Imperative commands: `create deployment, run, expose, scale`
- Imperative objects: `create -f file.yml`
- Declarative objects: `apply -f file.yml`

> Most important rule: Don't mix the three approaches. Stick to one (preferably Declarative)

Bret's recommendation:
1) Learn the Imperative command for easy control of local and test setups
2) Move to `apply -f file.yml` and `apply -f directory\` for prod
3) Store yaml in git, git commit each change before you apply.

***

## Generators
- Run, Create and Expose use helper templates called "generators".
- These generators are opinionated defaults.
- Every resources will have a spec which you can output:
```
kubectl create deployment sample --image nginx --dry-run -o yaml
```

Future of Run command:
- Will be similar like `docker run`
- Used for starting a Pod.
- Use `create` command for deployment

## Imperative vs Declarative
> Declarative programming is a programming paradigm that expresses the logic of a computation without describing its control flow. Imperative programming is a programming paradigm that uses statements that change a program’s state. ~ https://docs.microsoft.com/en-us/answers/questions/211050/declarative-vs-imperative-language.html#:~:text=Declarative%20programming%20is%20a%20programming,that%20change%20a%20program's%20state.

Imperative is good for learning but difficult to automate. It requires different commands to change the state.

Declarative -> We care only the end-state to be like the yaml contents. It is easier to automate. More suitable for DevOps

