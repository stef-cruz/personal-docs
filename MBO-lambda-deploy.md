# MBO lambda deploy

Once you merge your code from the kitman-lambda repo to master, it will create a new version of the build. 

The new version can be seen on CircleCI > search for you PR and click on rubuy lambdas. 


![Screenshot 2022-12-06 at 16 06 19](https://user-images.githubusercontent.com/109154890/205983801-1ab2bd50-6bcb-4975-8049-6cc4f6a4d211.png)

Get the version corresponding to the region (eu-west-1 code has to be used to update the eu-west-1 instances staging, production and sandbox).

Now we need to update the version in `elon` so it deploys the correct version.

Navigate to the directory dev/elon, cd into the folder where you updated the version in the file in the instance SANDBOX only, see if the version can be updated with the command kitman tg apply

create elon PR
The elon PRs need to be a bit more elaborated, go to the directories where the changes will be applied and run the code Kitman tg plan —local —pr (local because still not merged to master).

once the PR is merged then you can apply 
go to production directory and run command <kitman tg apply>
this pull the latest master in the container. Then go and add a comment with the result of this code in the PR. Look at old PRs to see the format. 
