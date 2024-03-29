# GitLFS-2022
Testing and template code for using Git large file storage
# Large file storage tips and tricks. 

To download Git LFS go to https://docs.github.com/en/repositories/working-with-files/managing-large-files/installing-git-large-file-storage. This resource is the most useful and will walk you through all associated steps and troubleshooting. However, I'll try to summarize the main commands here by working with this test repository. The repository contains a single large file (satellite derived kelp measures).  



Step 1. Clone the repository locally 

> git clone https://github.com/stier-lab/GitLFS-2022.git

Step 2. After downloading make a change to the test data file, save the file. Then commit and push.

> git add - A
> git commit -m "your message here."
> git push

You should receive some error like this:

remote: error: GH001: Large files detected. You may want to try Git Large File Storage - https://git-lfs.github.com.
To https://github.com/stier-lab/GitLFS-2022.git
 ! [remote rejected] main -> main (pre-receive hook declined)
error: failed to push some refs to 'https://github.com/stier-lab/GitLFS-2022.git'

This is a problem because now you have a large file tracked in your commit history that cannot be pushed to Github. Thus all changes in the repository are not being backed up to the cloud. A computer crash could result in the loss of work after the last successful push. 

Step 3. Git LFS has a command to determine which files are causing the error

> git lfs migrate info

This should return something like this: 

migrate: Fetching remote refs: ..., done.                                       
migrate: Sorting commits: ..., done.                                            
migrate: Examining commits: 100% (1/1), done.                                   
*.csv      	246 MB	1/1 file 	100%
*.txt      	720 B 	1/1 file 	100%
*.gitignore	570 B 	1/1 file 	100%
*.md       	73 B  	1/1 file 	100%

Github has a file size limit of 50 MB so any files larger will need to be managed by Git LFS. 

Step 4. Pluck the large file out of the commit history and track with Git LFS

The following command will take the large file, pluck it from the commit history and track with Git LFS

> git lfs migrate import --include="LargeDataFile/kelp_months.csv"

- !!!!! make sure that you have committed immediately prior to running this command. Any other changes to the repository, that were not committed will be lost upon running the command. But you will receive a warning if this is the case.

Step 5. Now push the repository. (This can take a little while, or at least longer than you would expect for you connections upload speeds)

> git push


# General comments

For each repository where you use git lfs you need to run: 

> git lfs install

This will initialize git lfs for the repository. If the repository already exists, you can then use 

> git lfs pull 

... this will allow you to seamlessly access the large files. 


