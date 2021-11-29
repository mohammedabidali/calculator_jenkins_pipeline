# Calculator test project

The aim of this project is to provide a project where tests can be run and reported in the command line.

# Tests

This is a basic calculator class with two very basic methods.

The tests have been written in both JUnit5 and JGiven.

## Setting JAVA_HOME

You will need to ensure JAVA_HOME is set correctly.

## Gradle & test

Simply running `gradle test` in the command line will run the tests and `gradle test -i` will give more verbose output.

There is no need to install gradle as everything is packaged within this project.

Test output can be found in:

**Build folder**

You will find an HTML report in the folder:

`build/reports/index.html`

you will also find `XML` result output in the folder:

`build/test-results/test/TEST-CalculatorUnitTests.xml`

`build/test-results/test/TEST-CalculatorJgiven.xml`

**JGiven reports**

you will find a JSON report for JGiven output:

`jgiven-reports/TestCalculatorJgiven.json`

# Setting up Jenkins CI
- Select `New Item` from the Jenkins dashboard
- Enter the name of the item and select `Freestyle project`. The name can be something like `Name-CI`
- In your new project page, select `Configure` to set up the CI for you project
### In `General`:
- Enter a description of what you are creating. E.g. 'Creating CI for calculator project'
- Tick the `Discard old builds box` and within it define the `Max # of builds to keep`. 3 is usually a good number for the amount of builds to keep. Too many builds can cause issues and crach the server.
- Tick the `GitHub project` and enter the https link to your project repository in github. E.g. https://github.com/mohammedabidali/calculator_jenkins_pipeline.git/
- skip `Office 365 Connector`
### In `Source Code Management`:
- tick the 'Git' box and it will expand to give you more options
- Under `Repositories` header:
- for `Repository URL` provide the ssh url from GitHub. E.g. git@github.com:mohammedabidali/calculator_jenkins_pipeline.git
- If you have previously created your ssh `credentials` then select it from the drop down menu, otherwise add a new key by clicking the `Add` option, and selecting `Jenkins` from the drop down menu. (only has Jenkins as an option anyway):
1. Within `Jenkins Credentials Provider: Jenkins`, select `SSH Username with private key` for the `Kind`, and add a short description of the key as well as provide a username so that you can distinguish your key.
2. Now paste your private key by ticking the `Enter directly` option under `Private Key` header.
3. Click `Add` to save and add your private ssh key.
- Finally, under the `Credentials` header, select your ssh key from the drop down menu. This key will allow Jenkins to ssh into GiHub automatically.
![](images/adding-ssh-key-to-jenkins.png)
- Under `Branches to build` header:
- for `Branches Specifier`, add the branch in you want jenkins to merge with the main in GitHub if it passes all the tests. E.g. `*/Dev`
- Leave everything else to default options
### In `Build Triggers`:
- select `GitHub hook triggers for GITScm polling`. Git hooks are scripts that run automatically every time a particular event occurs in a Git repository. Every time we push new code to the Dev branch, jenkins will be trigered to run tests, if they are set up and merge with main if we configure it that way. 
- skip `Build Environment`
### In `Build`:
- Select `Invoke Gradle scrips` from the options
- Tick `Use Gradle Wrapper` and then tick `Make gradlew executable` when it appears.
- Under `Task` type the name of task you want to run a script on. E.g. `test` (which is found within your files when pusing it to Dev branch on GitHub)
- Select `Add post-build action` and select `Build other projects` from the options
- Enter the name of the `Project to build`. This option can only be done after you have created a separate project. E.g. `Name-CI-merge`. This project will automatically start building after this first one finihsed bulding successfully
- Finally, click `Apply` and the click `Save`
- In the current project page, you can click `Build Now` to trigger the build manualy and check if it has been set up correctly.
- Under `Build History` you will see the build in real time. you can click on it to see it working. You can check the logs within the build to see if it has built successfully or if there are any errors that need to be resolved.
# Setting up Jenkins CI-Merge
