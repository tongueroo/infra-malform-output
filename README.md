# Terraspace Malform Output Debugging

Example project to debug: https://community.boltops.com/t/troubleshooting-error-stack-module-not-found/671

There's an intentional bug with the `output` helper in this code to see what happens when there's an invalid stack used in the output helper.

The code has an intentional error:

app/stacks/a1/tfvars/base.tfvars

    length1 = <= output("makebelievestack.length") %>

The `makebelievestack` does not exist. Only the `a1` and `b1` stacks exist.

    $ terraspace list
    app/stacks/a1
    app/stacks/b1
    $

## Debugging Session

    $ terraspace all up -y
    Building one stack to build all stacks
    Building .terraspace-cache/us-west-2/dev/stacks/b1
    WARN: The makebelievestack stack does not exist
    Here's the line in app/stacks/a1/tfvars/base.tfvars with the error:

    1 length1 = <%= output("makebelievestack.length") %>
    ERROR: stack makebelievestack not found
    $