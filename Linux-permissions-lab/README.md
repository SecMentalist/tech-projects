# Linux Permissions Lab – Security Analyst Report

**Date:** May 6, 2026  
**Scenario:** Examine and correct permissions in `/home/researcher2/projects` to align with least privilege.

---

## Task 1 – Check file and directory details

I start by navigating to the `projects` directory and listing all files, including hidden ones.

```bash
cd /home/researcher2/projects
ls -l
ls -la

Output:
text

researcher2@bdbeda604363:~$ cd /home/researcher2/projects
researcher2@bdbeda604363:~/projects$ ls -l
total 20
drwx--x--- 2 researcher2 research_team 4096 May  6 12:57 drafts
-rw-rw-rw- 1 researcher2 research_team   46 May  6 12:57 project_k.txt
-rw-r----- 1 researcher2 research_team   46 May  6 12:57 project_m.txt
-rw-rw-r-- 1 researcher2 research_team   46 May  6 12:57 project_r.txt
-rw-rw-r-- 1 researcher2 research_team   46 May  6 12:57 project_t.txt

researcher2@bdbeda604363:~/projects$ ls -la
total 32
drwxr-xr-x 3 researcher2 research_team 4096 May  6 12:57 .
drwxr-xr-x 3 researcher2 research_team 4096 May  6 13:25 ..
-rw--w---- 1 researcher2 research_team   46 May  6 12:57 .project_x.txt
drwx--x--- 2 researcher2 research_team 4096 May  6 12:57 drafts
-rw-rw-rw- 1 researcher2 research_team   46 May  6 12:57 project_k.txt
-rw-r----- 1 researcher2 research_team   46 May  6 12:57 project_m.txt
-rw-rw-r-- 1 researcher2 research_team   46 May  6 12:57 project_r.txt
-rw-rw-r-- 1 researcher2 research_team   46 May  6 12:57 project_t.txt

Findings:

    The group owner of all files is research_team.

    A hidden file exists: .project_x.txt.

Task 2 – Change file permissions (remove unauthorised write access)

The security policy states: None of the files should allow "other" users to write.

From the listing, project_k.txt has -rw-rw-rw- – others have write permission. That is a violation.

Remove write for others:
bash

chmod o-w project_k.txt

Verify:
text

researcher2@bdbeda604363:~/projects$ chmod o-w project_k.txt
researcher2@bdbeda604363:~/projects$ ls -la
total 32
drwxr-xr-x 3 researcher2 research_team 4096 May  6 12:57 .
drwxr-xr-x 3 researcher2 research_team 4096 May  6 13:25 ..
-rw--w---- 1 researcher2 research_team   46 May  6 12:57 .project_x.txt
drwx--x--- 2 researcher2 research_team 4096 May  6 12:57 drafts
-rw-rw-r-- 1 researcher2 research_team   46 May  6 12:57 project_k.txt   ✅ others write removed
-rw-r----- 1 researcher2 research_team   46 May  6 12:57 project_m.txt
-rw-rw-r-- 1 researcher2 research_team   46 May  6 12:57 project_r.txt
-rw-rw-r-- 1 researcher2 research_team   46 May  6 12:57 project_t.txt

Next, project_m.txt should be restricted – no group read or write. Initially it shows -rw-r----- (group has read). Remove group read:
bash

chmod g-r project_m.txt

Verification:
text

researcher2@bdbeda604363:~/projects$ chmod g-r project_m.txt
researcher2@bdbeda604363:~/projects$ ls -la project_m.txt
-rw------- 1 researcher2 research_team 46 May  6 12:57 project_m.txt

Now only the user can read and write – compliant.
Task 3 – Change file permissions on hidden file .project_x.txt

The hidden file .project_x.txt is archived. Policy: User and group may read, but no one may write.

Current permissions: -rw--w----

    User: read + write (violation – user should not write)

    Group: write only (violation – group should read, not write)

    Others: none

Set to r--r----- (user read, group read, others none):
bash

chmod u-w,g-w,g+r .project_x.txt

Verification:
text

researcher2@bdbeda604363:~/projects$ chmod u-w,g-w,g+r .project_x.txt
researcher2@bdbeda604363:~/projects$ ls -la .project_x.txt
-r--r----- 1 researcher2 research_team 46 May  6 12:57 .project_x.txt

✅ Correct. User and group can read, no write access.
Task 4 – Change directory permissions (drafts)

Requirement: Only researcher2 should be able to access the drafts directory. Remove group execute.

Check current permissions from projects parent directory:
bash

cd /home/researcher2/projects
ls -la

Output:
text

total 32
drwxr-xr-x 3 researcher2 research_team 4096 May  6 12:57 .
drwxr-xr-x 3 researcher2 research_team 4096 May  6 13:25 ..
-r--r----- 1 researcher2 research_team   46 May  6 12:57 .project_x.txt
drwx--x--- 2 researcher2 research_team 4096 May  6 12:57 drafts
-rw-rw-r-- 1 researcher2 research_team   46 May  6 12:57 project_k.txt
-rw------- 1 researcher2 research_team   46 May  6 12:57 project_m.txt
-rw-rw-r-- 1 researcher2 research_team   46 May  6 12:57 project_r.txt
-rw-rw-r-- 1 researcher2 research_team   46 May  6 12:57 project_t.txt

drafts has drwx--x--- – group has execute (violation).

Navigate into drafts:
bash

cd /home/researcher2/projects/drafts
ls -la

Output:
text

total 8
drwx--x--- 2 researcher2 research_team 4096 May  6 12:57 .
drwxr-xr-x 3 researcher2 research_team 4096 May  6 12:57 ..

Return to projects and remove group execute:
bash

cd /home/researcher2/projects
chmod g-x drafts

Verify:
bash

cd /home/researcher2/projects/drafts
ls -la

Output:
text

total 8
drwx------ 2 researcher2 research_team 4096 May  6 12:57 .
drwxr-xr-x 3 researcher2 research_team 4096 May  6 12:57 ..

Now drafts is drwx------ – group execute removed. Only researcher2 retains access.

✅ Requirement met.
