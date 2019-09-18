---
layout: post
title:  "Lambda Function Deployment"
categories: [AWS]
tags: [Lambda, Deploy]
read: 10 min
---

<p>We will learn how to write some simple shell script and execute it. This is a demonstration on how to deploy lambda function through AWS CLI, executing built script from EC2 instance. Before you going further in this read, please make sure you having enough understanding on [Lambda function Versioning]({% post_url 2019-08-10-lambda-version-alias %})</p>.


<p>Let's create this .sh for lambda function publish and update, targetting NodeJs Runtime for the time being. Also, please consider below scenario: </p>

*   We have previously created an PROD alias & STG alias for your lambda function.
*   Alias STG & PROD pointing to latest version of Lambda Function.
*   We have a git containing our lambda function source.
*   We have previously created an .env file in our S3 bucket. 
*   We are going to clone our lambda function source from git.
*   We are going to download our .env file. 
*   We have npm install in our EC2

***

#### Create a .sh file and name it deploy.sh

<p>Let's chunk this file into several parts.</p>

```sh
#!/bin/bash

scriptDir=$(pwd)

showHeader() {
    cat <<'EOF'
----------------------------------------------------------
    Lambda FunctionA, FunctionB Deployment Tool
----------------------------------------------------------

EOF
}

# Start. Clear screen..
clear

showHeader
```

##### Explaination

*   #!/bin/bash => It's a convention so the *nix shell knows what kind of interpreter to run
*   Clear screen
*   Show Starting header


```sh
#############################################################################
# Publish lambda function with this description
echo "Which project(s) to be published? [Choose Number]"
echo "1. FunctionA git Only."
echo "2. FunctionB git Only."
echo "3. FunctionA & FunctionB git."
echo -n "> "

read choice

echo ""

if [ "1" == "$choice" ]; then
    echo "Preparing to Publish FunctionA..."
    $scriptDir/functionA.sh

elif [ "2" == "$choice" ]; then
    echo "Preparing to Publish FunctionB..."
    $scriptDir/functionB.sh

elif [ "3" == "$choice" ]; then
    echo "Preparing to Publish FunctionA & FunctionB..."
    $scriptDir/functionA.sh
    $scriptDir/functionB.sh
else
    echo "Bad Choice"
    exit 1

fi
```

##### Explaination

*   Prompt user input, read input
*   Execute certain .sh file based on user's choice.

***

#### Let's create another functionA.sh file
```sh
#!/bin/bash

scriptDir=$(pwd)
#############################################################################
#  Get Current Lambda's Version for Acknowledgement 
#############################################################################
currentProdVersion=$(aws lambda get-alias \
  --function-name functionA \
  --name PROD \
  --query FunctionVersion \
  --output text) \
  
# Message :
echo "Current PROD version of functionA: "$currentProdVersion
```
##### Explaination

*   aws lambda .. represent cmd for AWS cli. 
*   Above AWS cli cmd is used to get the version of pointing alias of the lambda functionA.

```sh
#############################################################################
# Asking git branch for functionA
#############################################################################
echo "Which branch on functionA?"
echo -n "> "

#eg: master, featureA, featureB
read branchName

echo ""

if [ "" == "$branchName" ]; then
    echo "Invalid input. Please try again."
    echo ""
    exit 1
fi

#############################################################################
# Fetch git origin head
#############################################################################
echo "Fetching latest git version..."

#############################################################################
#  Clone git Repo
#  Download .env file from S3 
#  Zip For Ready Deploy 
#############################################################################

#Clone From your git source
git clone [your-git-source-url]
cd [your-project-souce-dir]

git config --get remote.origin.url

git checkout $branchName

checkBranch=$(git symbolic-ref --short HEAD)

if [ "${branchName}" == "${checkBranch}" ]; then
    echo "Branch name $branchName exists."

    echo "Source cloned completed."
```
##### Explaination

*   Asking for git branch for functionA source so that the branch would be deployed.  
*   Cloning of src will be done.
*   And finally cd into your project source dir

```sh
    # 3. Download .env file from S3 bucket && zip as functionA.zip 
    aws s3 sync s3://[your-functionA-env-file-path] ./

    echo ".env file downloaded."

    echo "start npm install..."
    npm install
```
##### Explaination

*   Download S3 .env file for functionA  
*   Start npm install for packages in package.json

```sh
    # Zip the current directory as a zip file namely functionA.zip
    zip -r $scriptDir/functionA.zip .
    cd $scriptDir

    echo "Project file zipped. Ready to deploy."

    #############################################################################
    # Publish lambda function with this description
    #############################################################################
    echo "What is the description for this version publish?"
    echo -n "> "

    read inputReleaseDesc

    echo ""

    if [ "" == "$inputReleaseDesc" ]; then
        echo "Invalid input. Please try again."
        echo ""
        exit 1
    fi

```
##### Explaination

*   Zip the current directory (including node_modules & .env file)  
*   Asking for publish description

```sh
    ##################################
    #  Update Lambda Function Codes  #
    ##################################
    aws lambda update-function-code \
    --function-name functionA \
    --zip-file fileb://functionA.zip \
    --no-publish

    #####################################
    #  Upgrade/Set Runtime to Node8.10  #
    #####################################
    aws lambda update-function-configuration \
    --function-name functionA \
    --runtime "nodejs8.10" \
    --timeout 30

    #####################################
    #  Publish Lambda Version           #
    #####################################
    aws lambda publish-version \
    --function-name functionA \
    --description "$inputReleaseDesc"

    latestVersion=$(aws lambda publish-version \
      --function-name functionA \
      --query Version \
      --output text)

    aws lambda update-alias \
    --function-name functionA \
    --function-version $latestVersion \
    --name STG
```
##### Explaination

*   Update Lambda functionA code with zipped file (functionA.zip)
*   Set Lambda functionA runtime as Node8.10
*   Publish Lambda functionA with previous input description
*   Set the STG alias to point to latest version 

```sh
    # 3. Clean Up
    rm -rf [your-project-souce-dir]
    rm functionA.zip 

    # Message :
    echo "functionA - Code Version Published. Latest version: "$latestVersion.
    echo "Alias STG pointing to version: "$latestVersion.
    echo "Alias PROD pointing to version: "$currentProdVersion.

else
    echo "Branch not exist"

    exit 1
fi
```

##### Explaination
*   Do clean up by removing zip file and source folder (download from git clone)

***

### Notes

![Octocat](https://github.githubassets.com/images/icons/emoji/octocat.png)

#### For the time being 


<p>Event Souce that invokes your Lambda functionA will have nothing change. Until you run an update from the aws cli with below command from your EC2 instance. (if $latestVersion showing version of 14)</p>

```sh
aws lambda update-alias --function-name functionA \
--function-version 14 --name PROD
```

##### Explaination
*   Update Lambda functionA's PROD alias to point to version 14 (latest version in this case)

##### You Are Good to GO!!