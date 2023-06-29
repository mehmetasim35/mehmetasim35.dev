---
layout: mypost
title: How to Fix Wrong Committer
categories: [Git]
tags: [Git,Gitlab,Github]
---


When you need to correct the committer information in your Git repository, follow these steps:

**Note**: Modifying commit history using `git filter-branch` and forcefully pushing changes can have significant consequences. It's important to exercise caution and understand the implications before proceeding.

Using `git filter-branch` and forcefully pushing changes can have several consequences:

- **Loss of Commit History**: Modifying commit information creates new commits with different metadata, effectively replacing the old commits and rewriting the commit history. This can make it difficult to track the original commits and associated information.

- **Collaboration Challenges**: Rewriting commit history can cause synchronization issues when multiple people collaborate on the same codebase. Other collaborators may have based their work on the original commits, leading to conflicts and difficulties merging their work with the updated history.

- **Disruption of Integrations**: Modifying commit history can disrupt integrations with various tools and services like continuous integration (CI) pipelines, issue trackers, and code review systems. These integrations rely on the original commits and their metadata for proper functioning.

To fix the wrong committer information, follow these steps:

1. Open your terminal and execute the following command:
```bash
#!/bin/sh
git filter-branch --env-filter '
OLD_EMAIL="<incorrect_email>"
CORRECT_NAME="<correct_name>"
CORRECT_EMAIL="<correct_mail>"
if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```

2.After running the command, execute the following command to forcefully push the updated commits and tags to your remote repository:
```bash
git push --force --tags origin 'refs/heads/*'
```

By following these steps, you will fix the wrong committer information in your Git repository. Ensure that you replace `<incorrect_email>`, `<correct_name>`, and `<correct_mail>` with the appropriate values specific to your situation.


