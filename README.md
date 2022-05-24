# github-actions-template
Templates for GitHub Actions
#
## Events that trigger workflows
Workflow triggers are events that cause a workflow to run.  These events can be:

+ Events that occur in your workflow's repository using event activity types & filters
+ Events that occur outside of GitHub and trigger a repository_dispatch event on GitHub
+ Scheduled times
+ Manual

## CI/CD Workflow

Develop templates to implement the different triggering mechanism for specific conditions supporting the Master Git Workflow as presented below:

<img src="images/ci-cd-workflow.png" alt="drawing" width="600"/>

## GitHub Action Workflow Templates

### Feature Branch Workflows

1. Feature Branch Push - No Actions   
   


```
> git add .
> git commit -m "My message.  [skip-actions]"
> git push 
```

2. Feature Branch Push - Code Scans (CVEs & SCA)     

```
> gh issue create --title "Feature - Scan Only" --body "Run all scans" --label "sca-only,sast-only" 
> git add .
> git commit -m "Fixed code -  #7"
> git push 
```

3. Feature Branch Push - Unit Test    

```
> gh issue create --title "Feature - Test Only" --body "Run all scans" --label "test-only" 
> git add .
> git commit -m "Run tests only"
> git push 
```

4. Feature Branch Push - Ready for Pull Request (PR) to Develop Branch

```
>gh issue create --title "Feature - All" --body "Run all scans and tests" --label "sca-only,sast-only,test-only" 
> git add .
> git commit -m "Run All Jobs"
> git push 
```

5. Merge Feature to Develop Branch

```
If you are alone working on FeatureB branch, the a pull --rebase develop is the best practice: you are replaying FeatureB changes on top of FeatureA. (and git push --force after).
```

### Develop Branch Workflows

1. Develop Branch Push - No Actions   

    In GitHub repo, switch to the develop branch:

```

```

2. Develop Branch Push - Code Scans (CVEs & SCA)   


3. Develop Branch Push - Full Scans & Tests  
    

4. BugFix Branch - Full Scans & Tests 


5. Develop Branch Push - Ready for Pull Request (PR) to Master Branch


6. Merge Develop to Master Branch

### Master Branch Workflows

1.  Release Branch Push - Ready for Pull Request (PR) to Master Branch


2.  Code Build - Validations


3.  HotFix Branch - Full Scans & Tests 