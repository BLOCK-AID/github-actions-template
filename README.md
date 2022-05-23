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


2. Feature Branch Push - No Actions   


3. Feature Branch Push - Code Scans (CVEs & SCA)     


4. Feature Branch Push - Unit Test    


5. Feature Branch Push - Ready for Pull Request (PR) to Develop Branch


6. Release Branch - Validations   


7. Release Branch Push - No Actions   


8. Release Branch Push - Code Scans (CVEs & SCA)   


9.  Release Branch Push - Automated Tests   
    

10. BugFix Branch - Full Scans &  Tests 


11. Release Branch Push - Ready for Pull Request (PR) to Master Branch


12. Code Build - Validations


13. HotFix Branch - Full Scans & Tests 