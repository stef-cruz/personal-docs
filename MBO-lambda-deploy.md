# MBO lambda deploy

This documentation has been posted [in the MBO lambda repo](https://github.com/KitmanLabs/kitman-lambda/tree/master/mindbody_integration)





Once you merge your code from the `kitman-lambda` repo to master, it will create a new version of the build. 

The new version number can be seen on CircleCI > navigate to kitman-lambda > search for you PR > click on Ruby lambdas and expand the dropdown that starts with `cd mindbody-integration`.

<img width="1613" alt="Screenshot 2022-12-07 at 09 47 17" src="https://user-images.githubusercontent.com/109154890/206145395-113f4741-e914-4f72-9c0d-cf3e6101fd7b.png">

The next step is to update the new versions in [`elon` repo](https://github.com/KitmanLabs/elon) for the staging, sandbox and production environments so it will deploy a new build with the version that contains your changes. Note: each region has a different version so make sure to update the `eu-west-1` version in the `eu-west-1` directory. 

![Screenshot 2022-12-06 at 16 07 53](https://user-images.githubusercontent.com/109154890/206143936-9947b6ea-a254-4294-ae52-a3e99d43ab06.png)

In `elon` repo, update the version for the corresponding region in all instances production, staging and sanbdox. `global/organizations/${INSTANCE}/${REGION}/api_gateway/integrations/terragrunt.hcl`.   
INSTANCE = production, staging or sandbox.   
REGION = eu-west-1, us-east-1, etc.

Before creating the PR, cd into the SANDBOX directory and run the command `kitman tg apply --local` and check the output to make sure that only your changes have been updated in this instance. Once that is done create the PR, [see an example here](https://github.com/KitmanLabs/elon/pull/2136).

The Elon PR requires the output of the planned changes applied to both production and staging. Navigate to the directory  `global/organizations/${INSTANCE}/${REGION}/api_gateway/integrations` and run the command `kitman tg plan —-local —pr`. The tag `--local` is there because the PR hasn't been merged to master yet. Paste the output in the PR for production & staging.

After the PR is reviewed and merged you can apply the changes. Navigate to the directory mentioned above and run command `kitman tg apply` which will pull the latest master branch in the container. Paste the output as a comment in the PR for prod and staging. Follow the format of [this PR](https://github.com/KitmanLabs/elon/pull/2136).
