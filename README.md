# Backport_tools
Provides support to export github comments from pre interactive rebase stage to post interactive rebase

# About

This directory contains git hooks for pre-rebase and post-rewrite

pre-rebase hook is useful during the initialization of interactive rebase.

post-rewrite hook can be invoked after rebase completion, git commit --amend, 
git cherry-pick etc..,

However, for this project, post-rewrite hook is designed to only work for 'rebase'.

# Steps

0. Add personal Github token to the GITHUB_TOKEN env variable. 
	
    export GITHUB_TOKEN=your_github_token

    Add the above line to ~/.bashrc, ~/.zshrc , or ~/.profile to make the
    change persistent. 

1. Move the files pre-rebase and post-rewrite hooks to .git/hooks/ directory.

2. Make these hooks executable: chmod +x pre-rebase, chmod +x post-rewrite

3. Select the commits for rebase. Example: 15 commits to be rebased.

    git rebase -i HEAD~15

5. Select actions for these commits (pick/reword/edit)

6. While these actions take place, pre-rebase hook makes necessary updates to 
   /tmp/comments.json. 

	open /tmp/comments.json 

     Run the above command to be able to see latest updates that are made to 
     comments.json while the interactive rebase is STILL ative. 

7. Once after rebase finishes. Post-rewrite hook gets triggered to perform git push
   to the upstream branch and thereafter all the comments are populated from old SHAs
   to New SHAs.

NOTE: Some commits in the rebase may NOT have comments. This will make post-rewrite
      trigger a warning saying that new SHA (after rebase) does not have a corresponding
      github comment, but that is Okay since old SHA did not have a corresponding github
      comment to begin with. 


Once after rebase finishes, all the review comments associated with old SHAs are now
exported to new SHAs. This will help reviewers to quickly look back at their earlier
review comments and ensure those comments are all addressed in the current iteration.

Please reach out to Pavan Kumar Paluri <pavankumar.paluri@amd.com> in case of any queries.
