==========================  main.yml ========================== 

Ok, what does all this code do? Let’s break it down.

First, we define the name of this action. You’ll use this to identify the workflow if you have many. Next, we define trigger with on: [push]. This workflow will run anytime you push code to the repo.

We only define one job called build and deploy lambda. It uses node version 12.x and it runs on the latest version of Ubuntu. Every time the build starts you’ll get a fresh install of Ubuntu.

The main part of the config is under the steps: key. Each step starts either with a uses: or a name:. I’ll go through all the steps here:

actions/checkout@v1 This step checks out the code from a Github repo.
Use Node.js ${{ matrix.node-version }} This step installs node on the fresh Ubuntu machine.
npm install and build This step runs npm ci and then npm run build. This is useful if you want NPM dependencies in your Lambda in the future.
zip zip down everything to a file called bundle.zip
default deploy deploy the newly created bundle.zip to the Lambda named my-function (change the name if you chose a different name when you created your Lambda)
In the script we reference secrets.AWS_ACCESS_KEY_ID and secrets.AWS_SECRET_ACCESS_KEY. The reason we don’t just hardcode the keys here is because of security. Github has a built-in system to manage keys that we’ll use. 

# Steps

Go to Settings->Secrets in your Github repo and then add the secrets there.

Go to IAM page in your Amazon account. press users, and select your user. Press security credentials. Then use one of your existing access keys and secrets, or press “Create access key” to create a new one.

Then add the keys to your Github Secrets.

Now you are good to go. Commit and push the changes and the build will automatically start. Go to the Actions tab again to see the status.

Now when you go back to your AWS Lambda console and refresh it, you’ll see the code from your Github repo in there! And every time you push new code, it’ll be auto deployed.

This app can now be extended to use NPM dependencies, multiple files, unit-tests, and everything you need for a real-world app. What will you create?

Source: https://blog.jakoblind.no/aws-lambda-github-actions/