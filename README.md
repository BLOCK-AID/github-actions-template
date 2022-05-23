# github-actions-template
Templates for GitHub Actions
#
## Events that trigger workflows
Workflow triggers are events that cause a workflow to run.  These events can be:

+ Events that occur in your workflow's repository using event activity types & filters
+ Events that occur outside of GitHub and trigger a repository_dispatch event on GitHub
+ Scheduled times
+ Manual

## GitHub Action Workflow Templates

Develop templates to implement the different triggering mechanism for specific conditions within the Master Git Workflow.  

1. Feature Branch - Validations   

    Upon the creation of the repository containing both the master and develop branch, create an issue with a ToDo statement to validate the state of the repos.
```
# create new bug label
$ gh label create bug --description "Something isn't working" --color E99695
```


2. Feature Branch Push - No Actions   

```
> git add .
> git commit -m "My message.  [skip-actions]"
> git push 
```


3. Feature Branch Push - Code Scans (CVEs & SCA)     

```
> gh issue create --title "Feature - Scan Only" --body "Run all scans" --label "sca-only,sast-only" 
> git add .
> git commit -m "Run scans only"
> git push 
```

4. Feature Branch Push - Unit Test    

```
> gh issue create --title "Feature - Test Only" --body "Run all scans" --label "test-only" 
> git add .
> git commit -m "Run tests only"
> git push 
```

5. Feature Branch Push - Ready for Pull Request (PR) to Develop Branch

```
>gh issue create --title "Feature - All" --body "Run all scans and tests" --label "sca-only,sast-only,test-only" 
> git add .
> git commit -m "Run All Jobs"
> git push 
```


6. Release Branch - Validations   


7. Release Branch Push - No Actions   


8. Release Branch Push - Code Scans (CVEs & SCA)   


9.  Release Branch Push - Automated Tests   
    

10. BugFix Branch - Full Scans &  Tests 


11. Release Branch Push - Ready for Pull Request (PR) to Master Branch


12. Code Build - Validations


13. HotFix Branch - Full Scans & Tests 