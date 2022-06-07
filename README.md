# terraform-unlock-state-gh-action
Repository containing Ohpen's Github Action to unlock a Terraform state file following Ohpen standard. Include this in your repository and execute it manually (`on.workflow_dispatch`) providing the `LOCK_ID`. 

- [terraform-unlock-state-gh-action](#terraform-unlock-state-gh-action)
  - [code-of-conduct](#code-of-conduct)
  - [github-action](#github-action)

## code-of-conduct

Go crazy on the pull requests :) ! The only requirements are:

> - Use _conventional-commits_.
> - Include _jira-tickets_ in your commits.
> - Create/Update the documentation of the use case you are creating, improving or fixing. **[Boy scout](https://biratkirat.medium.com/step-8-the-boy-scout-rule-robert-c-martin-uncle-bob-9ac839778385) rules apply**. That means, for example, if you fix an already existing workflow, please include the necessary documentation to help everybody. The rule of thumb is: _leave the place (just a little bit)better than when you came_.

### github-action

This action performs a [_terraform force-unlock_](https://www.terraform.io/cli/commands/force-unlock) on the IaC that is specified. The (required) inputs are:

- _region_: aws region name.
- _access-key_: user access key to be used.
- _secret-key_: user secret key to be used.
- _terraform-folder_: folder where the terraform configuration is.
- _backend-configuration_: path of the tfvars file with backend configuration.
- _lock-id_: LOCK_ID for the terraform force-unlock command.

⚠️ Attention! Terraform will try to assume the deployment role in the destination AWS account. Such deployment will fail if the user is not allowed to assume such role.

Here is an example :

```yaml
name: tf-unlock-state
on:
  workflow_dispatch:
    inputs:
      lock_id:
        description: 'lock_id parameter to be used in the terraform force-unlock. e.g. 4822a3d3-f330-1h28-1aa3-d04458a06087'
        required: true 
        type: string 
jobs:
  unlock-tf-state-team-branch-deployment:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: ohpensource/terraform-unlock-state-gh-action@v0.0.3
        name: terraform plan
        with:
          region: $REGION
          access-key: $COR_AWS_ACCESS_KEY_ID
          secret-key: $COR_AWS_SECRET_ACCESS_KEY
          terraform-folder: "terraform"
          backend-configuration: "backend.tf"
          lock-id: $lock_id
```

Please note this is similar to the next gh actions:
* [terraform-plan-gh-action](https://github.com/ohpensource/terraform-plan-gh-action)
* [terraform-apply-gh-action](https://github.com/ohpensource/terraform-apply-gh-action)