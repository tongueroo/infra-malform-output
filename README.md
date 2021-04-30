# Terraspace Malform Output Debugging

Example project to debug: https://community.boltops.com/t/troubleshooting-error-stack-module-not-found/671

There's an intentional bug with the `output` helper in this code to see what happens when there's an invalid stack used in the output helper.

The code has an intentional error. It should be `<%=` not `<=`

app/stacks/a1/tfvars/base.tfvars

    length1 = <= output("b1.makebelieveoutput") %>

The `makebelieveoutput` does not exist. Only the `pet` and `length` outputs exist.

    $ cat app/stacks/b1/outputs.tf
    output "pet" {
      description = "pet"
      value       = random_pet.this.id
    }
    output "length" {
      description = "length"
      value       = 2
    }
    $

## Debugging Session

    $ terraspace all up -y
    Building one stack to build all stacks
    Building .terraspace-cache/us-west-2/dev/stacks/b1
    Downloading tfstate files for dependencies defined in tfvars...
    Built in .terraspace-cache/us-west-2/dev/stacks/b1
    Running:
        terraspace up b1 # batch 1
        terraspace up a1 # batch 2
    Batch Run 1:
    Running: terraspace up b1 Logs: log/up/b1.log
    terraspace up b1:  No changes. Infrastructure is up-to-date.
    terraspace up b1:  Apply complete! Resources: 0 added, 0 changed, 0 destroyed.
    Batch Run 2:
    Running: terraspace up a1 Logs: log/up/a1.log
    terraspace up a1:  Error: Invalid value for input variable
    Error running: terraspace up a1. Check logs and fix the error.
    $ cat .terraspace-cache/us-west-2/dev/stacks/a1/1-base.auto.tfvars
    length1 = "(Output makebelieveoutput was not found for the a1 tfvars file. Either b1 stack has not been deployed yet or it does not have this output: makebelieveoutput)"
    $
