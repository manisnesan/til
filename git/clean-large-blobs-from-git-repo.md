# Clean large blobs from git repository

## Problem Scenario

One of the developer mistakenly added 5 large files that are greater than 500MB in git repository and pushed them to the remote branch.
Realizing the mistaken they removed the files in the following commit. But this will not remove the file completely from git objects stored and it will still cause cloning repository and continuous integration to be painfully long.

## Options

I reviewed and narrowed down couple of options

1. git-filter-branch (extremely slow and have safety issues see the man page for WARNING)
2. bfg repo-cleaner (alternative and the option I went with due to the speed and performance)

## Background

Following are from References [3]

**Why is changing history such a tricky thing to do?**

A key feature in Git is simply, we always get out what we put in. To guarentee this the id for every object stored in Git (file, folder, or commit) is a strong hash derived from the contents of that object itself.

You can read about the Git object model in detail on git-scm.com[2].

### Key points

- Each specific version of a file, or folder, or commit is an object in Git.
- Each object is assigned an immutable hash-id which exactly identifies its contents.
- Changing the object slighlty, its id would change completely.
- Identical objects will have same ids and only one copy will ever be stored.
- The id of a commit is particularly precise. Commit id depends not only on its content, but also on the ids (and thus content) of all the commits that came before it.
- The id of a commit embodies its entire history.
- Changing any commit in Git history means creating a new copy of every single commit that comes after it.
- All of the commits need to reference the updated commit id of their parent. It has to be done sequentially, going from oldest commit to newest
- Clean commit cannot be created without first having the commit id of its cleaned parent.

## Solution

Warning: Ensure no inflight MR and the latest commit is clean.

Following the steps described using [bfg-repo-cleaner](https://rtyley.github.io/bfg-repo-cleaner/)[3] , we can remove the blobs of greater than 5M, this will likely rewrite the history with new commit ids. Hence all the developers & data scientists need to get a fresh copy of the repository post this change.

- `$ git clone --mirror -v $REPOSITORY_URL/garlock.git`
- `$ cd $PATH_TO_garlock.git && java -jar ~/Downloads/bfg-1.14.0.jar --strip-blobs-bigger-than 5M .`
- See the output report of step 2 below

```bash
$ java -jar ~/Downloads/bfg-1.14.0.jar --strip-blobs-bigger-than 5M .              
                                                                                                                    
Using repo : /tmp/garlock.git/.                                                                                     
                                                                                                                    
Scanning packfile for large blobs: 401                                                                              
Scanning packfile for large blobs completed in 37 ms.                                                               
Found 9 blob ids for large blobs - biggest=438073673 smallest=26372618                                              
Total size (unpacked)=464446291                                                                                     
Found 41 objects to protect                                                                                         
Found 34 commit-pointing refs : HEAD, refs/heads/master, refs/heads/msivanes_baseline, ...                          
                                                                                                                    
Protected commits                                                                                                   
-----------------                                                                                                   
                                                                                                                    
These are your protected commits, and so their contents will NOT be altered:                                        
                                                                                                                    
 * commit 40496a07 (protected by 'HEAD')                                                                            
                                                                                                                    
Cleaning                                                                                                            
--------                                                                                                            
                                                                                                                    
Found 87 commits                                                                                                    
Cleaning commits:       100% (87/87)                                                                                
Cleaning commits completed in 119 ms.                                                                               
                                                                                                                    
Updating 4 Refs                                                                                                     
---------------                                                                                                     
                                                                                                                    
        Ref                                 Before     After                                                        
        -------------------------------------------------------                                                     
        refs/heads/nvazquez_PNTBIZIN-5582 | ce31b2c5 | f81cb442                                                     
        refs/heads/temp_master            | f60834f1 | 82c0a312                                                     
        refs/merge-requests/10/head       | ce31b2c5 | f81cb442                                                     
        refs/merge-requests/10/merge      | 013e2af6 | 4516432c                                                     
                                                                                                                    
Updating references:    100% (4/4)                                                                                  
...Ref update completed in 15 ms.                                                                                   
                                                                                                                    
Commit Tree-Dirt History                                                                                            
------------------------                                                                                            
                                                                                                                    
        Earliest                                              Latest
        |                                                          |
        ................................................D...DDD.D.D.

        D = dirty commits (file tree fixed)                                                                         
        m = modified commits (commit message or parents changed)
        . = clean commits (no changes to file tree)                                                                 

                                Before     After                                                                    
        -------------------------------------------                                                                 
        First modified commit | 20a60512 | 8aed45c7                                                                 
        Last dirty commit     | 013e2af6 | 4516432c                                                                 

Deleted files                                                                                                       
-------------                                                                                                       

        Filename              Git id                                   
        --------------------------------------------------------------
        pytorch_model.bin   | 0dbefb30 (417.8 MB), 2a6e38ec (417.8 MB)
        train_data.pt       | 9648b0ae (25.2 MB)                       
        train_epoch_0_sd.pt | 88bf1251 (417.8 MB), 03f20420 (417.8 MB)
        train_epoch_1_sd.pt | d85986a7 (417.8 MB), eb52c379 (417.8 MB)
        train_epoch_2_sd.pt | 9a009386 (417.8 MB), a216af91 (417.8 MB)
        train_epoch_3_sd.pt | 0dbefb30 (417.8 MB), 2a6e38ec (417.8 MB)


In total, 21 object ids were changed. Full details are logged here:

        /tmp/garlock.git/..bfg-report/2021-05-18/11-55-58
```

- `$ git reflog expire --expire=now --all && git gc --prune=now --aggressive`
- Disable the "Protected" branches setting in GITLAB/GITHUB to allow for pushing the rewritten commits.
- `$ git push`
- Developers need to delete the old copy of the repository and get a fresh copy of the repository.

## References

- [1] [Linus Tech Talk](https://youtu.be/4XpnKHJAok8)
- [2] [Git Object Model](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects)
- [3] [bfg-repo-cleaner](https://rtyley.github.io/bfg-repo-cleaner/)
