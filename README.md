# AOP for IaC PoC

This repo contains the code mentioned in my Medium article
"Musings on IaC":

https://medium.com/@d5b10/musings-on-iac-48dbc75d8832

These files try to implement Aspect-Oriented Programming (AOP) for
Infrastructure as Code (IaC). It is a proof of concept (PoC) which
transforms a given AWS Cloudformation template with an ECS service to
enable the execute-command feature of ECS, which requires a couple of
changes.  The trasformation is done with Jay Kuri's Data Tranformation
Language (DTL).

# The Code

The "out" transformation in `enable_ecs_execute_command.json` flattens
the input (the CF template in ecs-example.yaml) and concatenates other
flattened data structures which either add resources or attributes or
change previously defined attributes. Finally the result is unflattened
again to yield a valid Cloud Formation template.

I was too lazy to use Cloudformation parametes, so you need to go 
through ecs-example.yaml and replace all the placeholders (XYZ)
with your VPC, subnet and security group IDs.

You can run the example after installing DTL and then calling:

```
./node_modules/.bin/dtl -f enable_ecs_execute_command.json ecs-excample.yaml -O yaml > out.yaml
```

The resulting out.yaml can be deployed with CloudFormation.

# Problems

How to follow chains, e.g.: are there services which have task definitions
which have no task roles

# Author

Daniel Boesswetter <daniel@daniel-boesswetter.de>
