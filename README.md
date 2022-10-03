# github-actions-template
Triggers for GitHub Actions with Reusable Workflows

## Events that trigger workflows
Workflow triggers are events that cause a workflow to run.  These events can be:

+ Events that occur in your workflow's repository using event activity types & filters such as:
    + Pull Request
    + Semantic versioning (tags): major.minor.patch
    + Labels
+ Events that occur outside of GitHub and trigger a repository_dispatch event on GitHub
+ Scheduled times (cron)
+ Manual
## Prerequisites
### Secrets
You need to add the following secrets to GitHub Settings under Organization Secrets (recommended) for that repos:

+ SONARQUBE_HOST
+ SONARQUBE_TOKEN

### GitHub CLI
In order to get finer grain control of GitHub Pull Request from the command line, we need to get GitHub CLI, or gh, which is a command-line interface to GitHub for use in terminal or scripts.
It could be installed on Windows, Linux, and Mac as per:

Refs:
    + https://cli.github.com/manual/
#
# CI/CD Workflow
A pipeline as code file specifies the stages, jobs, and actions for a pipeline to perform. Because the file is versioned, changes in pipeline code can be tested in branches with the corresponding application release.

+ The pipeline as code model of creating continuous integration pipelines and continuous deployment is an industry best practice addressing the following pain points: 
+ Auditing was limited to what was already built-in
+ Developers and Operations Team were unable to collaborate
+ Troubleshooting problems across applications and infrastructure was difficult
+ Difficult to rollback changes to the last known configuration
+ Pipelines prone to breaking

A high level diagram illustrating the different activities across a CI/CD Pipeline is depicted herein:

<img src="images/GitHub-GitOps.png" alt="drawing" width="600"/>

This diagram delineates the different actions executed by the PBS DevSecOps teams, they are:

+ Development Team (Dev) responsible for:
    + Code creation and management for all applications
    + Unit & Integration Testing
    + Generation of artifacts (binaries and docker images)

+ Security Team (Sec) responsible for:
    + Establishing security policies and supporting tools
    + Constantly monitoring for vulnerabilities from development to production
    + Responsible for addressing security breaches
    + Manage the Software Bill of Material (SBoM) and CVE advisories

+ Operations Team (Ops)  responsible for:
    + Create virtual servers and networking across all environments
    + Deploy, manage, and monitor kubernetes clusters

These activities are managed under the teams' respective workflows that are also dovetailing on each other to provide maximum coverage throughout the enterprise.  For this project, only the Development (GitHub Actions) part of the CI/CD pipeline is presented.
#
### GitHub Actions - Continuous Integration
Develop templates to implement the different triggering mechanism for specific conditions supporting the Master Git Workflow as presented below:

<img src="images/GitHub-Actions-Workflow.png" alt="drawing" width="600"/>

This pipeline essentially covers all aspect of code creation, testing, compilation, and version management.  So our development pipeline must be able to automate as much as possible of the following GitHub Workflow:
#

# PBS GitHub Action Workflow Triggers
Workflow triggers are events that cause a workflow to run without manual inetrvention.

## Feature Branch Workflows
### Start with the develop branch
```
> git checkout develop
> git fetch origin 
> git reset --hard origin/develop
```
### Create a new feature branch
```
> git checkout -b feature-new
```
### Pull, update, add, commit, and push - No action
It is a good practice to pull when starting work for the day and push at the end of it.
The idea in saving youe work without a PR being triggered stem from the fact that no new tags
have been created
```
> git pull
> git status
> git add <some-file>
> git commit
> git push -u origin feature-new
```
### Feature Branch Detailed Actions
1. Feature Branch Push - Code Scans (SCA & SAST)     
```
> gh pr edit 13 --add-label "sca-only,sast-only"
> git add .
> git commit -m "Fixed code - perform SCA & SAST scans"
> git push 
```

2. Feature Branch Push - Unit Test    
```
> gh pr edit 13 --remove-label "sca-only,sast-only"
> gh pr edit 13 --add-label "test-only"
> git add .
> git commit -m "Fixed code - perform test"
> git push 
```

3. Feature Branch Push - Ready for Pull Request (PR) to Develop Branch
```
> gh pr edit 13 --remove-label "test-only"
> gh pr edit 13 --add-label "all-jobs"
> git add .
> git commit -m "Fixed code - execute all jobs from workflow"
> git push  
```

4. Merge Feature to Develop Branch - Approve Pull Request (PR) to Develop Branch via GUI or CLI.
```
If you are alone working on FeatureB branch, the a pull --rebase develop is the best practice: you are replaying FeatureB changes on top of FeatureA. (and git push --force after).
```

## Develop Branch Workflows
Nobody can make changes directly against the develop branch
except running tests and merging code during PRs.

### Develop Branch Detailed Actions
1. PR from Feature Branch - Runn all jobs in development branch
```
> git add .
> git commit -m "Fixed code - execute all jobs from workflow"
> git push  
```
2. SUCCESS - Ready for Pull Request (PR) to Release Branch
```
> gh pr edit 13 --remove-label "all-jobs"
> gh pr edit 13 --add-label "release"
> git add .
> git commit -m "Release code - open ticker in JIRA"
> git push  
```
3. FAILED - Create bugfix branch

## Release Branch Workflow
No work  is done directly on the release branch

### Create a new release branch from develop branch
```
> git checkout -b release-new
```

### Release Branch Detailed Actions

TBD


## Bugfix Branch Workflow
### Create a new bugfix branch
```
> git checkout -b bugfix-new
```
### Pull, update, add, commit, and push - No action
It is a good practice to pull when starting work for the day and push at the end of it.
The idea in saving youe work without a PR being triggered stem from the fact that no new tags
have been created
```
> git pull
> git status
> git add <some-file>
> git commit
> git push -u origin bugfix-new
```
### Bugfix Branch Detailed Actions
1. Bugfix Branch Push - Code Scans (SCA & SAST)     
```
> gh pr edit 13 --add-label "sca-only,sast-only"
> git add .
> git commit -m "Fixed code - perform SCA & SAST scans"
> git push 
```

2. Bugfix Branch Push - Unit Test    
```
> gh pr edit 13 --remove-label "sca-only,sast-only"
> gh pr edit 13 --add-label "test-only"
> git add .
> git commit -m "Fixed code - perform test"
> git push 
```

3. Bugfix Branch Push - Ready for Pull Request (PR) to Develop Branch
```
> gh pr edit 13 --remove-label "test-only"
> gh pr edit 13 --add-label "deploy"  // or minor version update (TBD)
> git add .
> git commit -m "Fixed code - execute all jobs from workflow"
> git push  
```

4. Merge Feature to Develop Branch - Approve Pull Request (PR) to Develop Branch via GUI or CLI.

## Master Branch Workflow
Nobody can make changes directly against the master branch.

1.  Master Branch - Full Build from PR (develop or hotfix branches)
```
> git add .
> git commit -m "Fixed code -  [all-jobs]"
> git push  
```

## Hotfix Branch Workflow
1. Hotfix Branch Push - Code Scans (SCA & SAST)     
```
> gh pr edit 13 --add-label "sca-only,sast-only"
> git add .
> git commit -m "Fixed code - perform SCA & SAST scans"
> git push 
```

2. Hotfix Branch Push - Unit Test    
```
> gh pr edit 13 --remove-label "sca-only,sast-only"
> gh pr edit 13 --add-label "test-only"
> git add .
> git commit -m "Fixed code - perform test"
> git push 
```

3. Hotfix Branch Push - Ready for Pull Request (PR) to Master Branch
```
> gh pr edit 13 --remove-label "test-only"
> gh pr edit 13 --add-label "deploy"
> git add .
> git commit -m "Fixed code - execute all jobs from workflow"
> git push  
```