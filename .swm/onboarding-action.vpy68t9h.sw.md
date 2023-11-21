---
id: vpy68t9h
title: Onboarding Action
file_version: 1.1.3
app_version: 1.18.34
---

This code snippet sets up an onboarding workflow for a new team member. It uses the `on` keyword with the `workflow_dispatch` event to trigger the workflow manually. The workflow is designed to provide a way to onboard new team members, but the specific functionality is not provided in the code snippet.
<!-- NOTE-swimm-snippet: the lines below link your snippet to Swimm -->
### 📄 .github/workflows/onboarding.yml
```yaml
1      name: Onboarding new team member
2      on:
3        workflow_dispatch:
4        
```

<br/>

This code snippet creates a job called "create-issues" that runs on an Ubuntu system. The job has permissions to write issues.
<!-- NOTE-swimm-snippet: the lines below link your snippet to Swimm -->
### 📄 .github/workflows/onboarding.yml
```yaml
5      jobs:
6        create-issues:
7          runs-on: ubuntu-latest
8          permissions:
9            issues: write
```

<br/>

This code snippet performs two actions.

First, it uses the `actions/checkout` action to check out the code from the repository. This action is typically used at the beginning of a workflow to fetch the latest code changes.

Next, it uses the `actions/github-script` action with version 6. This action allows you to run arbitrary JavaScript or TypeScript code using the GitHub API. In this case, the `script` input is empty, so no specific functionality is provided in the code snippet.
<!-- NOTE-swimm-snippet: the lines below link your snippet to Swimm -->
### 📄 .github/workflows/onboarding.yml
```yaml
10         steps:
11           - uses: actions/checkout@v3
12           
13           - uses: actions/github-script@v6
14             with:
15             github-token: ${{ github.token }}
16               script: |
```

<br/>

This code snippet reads the contents of multiple directories and files, extracts information from the file contents, and performs some operations. It retrieves a list of directories using the `fs.readdirSync` function, and then iterates over each directory. Inside each directory, it retrieves a list of files using `fs.readdirSync` and iterates over each file. It reads the contents of each file using `fs.readFileSync` and extracts the title from the file contents. It then checks if there is an open issue with the same title using a loop over the `issues` array. If a matching issue is found, it logs a message and returns. If no matching issue is found, it logs a message with the trigger actor, and continues to process the file contents by extracting the body and labels.
<!-- NOTE-swimm-snippet: the lines below link your snippet to Swimm -->
### 📄 .github/workflows/onboarding.yml
```yaml
15               script: |
16                 
17                  let fs = require('fs')             
18                  var directories = fs.readdirSync('issues/');
19                  
20                  const owner = context.repo.owner
21                  const repo = context.repo.repo
22                  let assignees = []
23                  assignees.push(context.actor)
24                  const opts = await github.rest.issues.listForRepo({
25                     owner,
26                     repo,
27                     assignees
28                  });
29                  const issues = await github.paginate(opts)
30                               
31                  directories.forEach(function (directory) {                
32                     console.log(directory);
33                     
34                     const files = fs.readdirSync(`issues/${directory}`, 'UTF8')
35                     
36                     files.forEach(function (file) {
37                     
38                       const fileContents = fs.readFileSync(`issues/${directory}/${file}`, 'UTF8')   
39                       let arr = fileContents.split('\n');
40                       const title = arr[1].substring(8, arr[1].length-1)
41                       console.log('title: ' + title)
42     
43                       // Check if there is an issue with same title
44                       for (const issue of issues) {
45                         if (issue.state === 'open' && issue.title === title) { // todo: remove the state check in production
46                           console.log(`Issue with title ${title} already exist for user ${context.actor}`)
47                           return 0
48                         }
49                       }
50     
51                       console.log(`Triggered by ${context.actor}`)
52     
53                       console.log('--------------------')
54                       let body = ''
55                       for (let i=3; i<arr.length; i++) {
56                           console.log(`${i} - ${arr[i]}`)
57                           body += arr[i] + '\n'
58                       }      
59                       console.log('-------BODY---------')
60                       console.log(body)
61                       
62                       let labels = []
63                       labels.push(directory)
64                       labels.push(`on-boarding`)
65     
66                       console.log('--------------------')
67     
68                       github.rest.issues.create({
69                             owner,
70                             repo,
71                             title,
72                             body,
73                             assignees,
74                             labels
75                           });
76                       });  
77                     }
78                   )
```

<br/>

This file was generated by Swimm. [Click here to view it in the app](https://app.swimm.io/repos/Z2l0aHViJTNBJTNBY29sbGFib3JhdGUtZWZmZWN0aXZlbHklM0ElM0F0aW0td2hpdGUtZXNyaQ==/docs/vpy68t9h).
