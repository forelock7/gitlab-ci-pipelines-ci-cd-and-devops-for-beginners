# Section 1: Introduction to GitLab CI/CD. Fundamental CI/CD concepts & DevOps

## 1. Introduction to the GitLab CI/CD course

## 2. What is GitLab CI/CD?

## 3. GitLab.com account registration

- https://medium.com/devops-with-valentine/setup-gitlab-ci-runner-with-docker-executor-on-windows-10-11-c58dafba9191
- https://www.youtube.com/watch?v=HGJWMTNeYqI

Create yopur own gitlab account:

- https://about.gitlab.com/

My account https://gitlab.com/forelock

## 4. Verifying your GitLab account

- https://gitlab.com/forelock/gitlab-for-beginners

## 5. What is a pipeline?

## 6. Your first pipeline

- YAML Syntax: https://learn.getgrav.org/16/advanced/yaml
- Top 10 Bash file system commands you can’t live without: https://hackernoon.com/top-10-bash-file-system-commands-you-cant-live-without-4cd937bd7df1

Created repository: https://gitlab.com/forelock/car-assembly

## 7. Help! My GitLab CI pipeline is not running.

## 8. Adding pipeline stages

Repository: https://gitlab.com/forelock/car-assembly

## 9. GitLab CI/CD architecture

- GitLab Server:
  - Stores the results;
  - Manages the pipelines and the associated jobs;
  - Coordinates the work but delegates the execution
- GitLab Runner:
  - Executes the job

## 10. GitLab CI/CD architecture (Part 2)

## 11. How much does GitLab cost?

- https://medium.com/devops-with-valentine/setup-gitlab-ci-runner-with-docker-executor-on-windows-10-11-c58dafba9191
- https://www.youtube.com/watch?v=HGJWMTNeYqI

## 12. What is a Linux shell?

- https://medium.com/devops-with-valentine/how-to-install-ubuntu-desktop-on-a-virtual-machine-using-virtualbox-7-7ee537b609b9

## 13. Writing comments in a GitLab pipeline

- "# Comment"

## 14. Manually stopping a pipeline

- Use UI buttons for job

## 15. Brief introduction to YAML

## 16. Safeguarding CI pipelines with effective tests

## 17. Why do jobs and pipelines fail (exit codes)

- Exit code: 0 - PASS
- Exit code: 1-255 - FAIL - immediately interapt

Example: sh"exit 1"

## 18. Important skills you need to acquire

## 19. What is DevOps?

## 20. Conclusion

# Section 2: Continuous Integration (CI) with GitLab (Stages, Tests, Reports, Merge Requests)

## 21. Introduction to Continuous Integration (CI)

- Git for GitLab (Beginner's FULL COURSE)[https://www.youtube.com/watch?v=4lxvVj7wlZw]

git --version
mkdir git-study
cd git-study
git init
git status
git config --global user.name "Valentin Despa"
git config --global user.email "valentin.despa@example.com"
vim readme.md
git add readme.md
git status
git commit -m "My first commit"
git status
git add . --- add to stage all files

Staging is usefull for check what ind of files we want to commit
git reset HEAD readme.md --- unstage readme.md file only to omit commiting it
git log --- logging commits history
git log --patch --- with more information

Git doesn't track folders. If you need to track it add file inside ".gitkeep"
touch temp/.gitkeep
git status

git checkout -b feature/new-table --- create new branch and checkout
git branch -d feature/new-table --- delete branch

git checkout master
git merge feature/new-table --- if there are no any changes in master, merge automaticaly will be executed with 'fast-forward' startegy
git log
git branch

If if there are some changes in master, merge will be executed with other startegy and require merge commit

git checkout bugfix/table-2
git rebase master

git merge --abort
git rebase --abort

Resolve conflick will create new commit as a result.

## 22. Forking a Git repository

Need to fork project: https://gitlab.com/gitlab-course-public/learn-gitlab-app
My forked project: https://gitlab.com/forelock/learn-gitlab-app

## 23. Running the project locally (optional)

- https://nodejs.org/en/download
- https://code.visualstudio.com/
- https://git-scm.com/
- https://www.youtube.com/watch?v=iXuIp5uNnLk
- https://www.youtube.com/watch?v=Vmt0V6a3ppE
- https://www.youtube.com/watch?v=_qDJ0W1wR5w

npm i
npm run build --- to build(bundle) the project
npm install -g serve --- install globally simple web-server to test project locally
serve -s build/

## 24. Using Docker as a build environment

test_npm:
image: node:22
script: - node --version - npm --version

## 25. Choosing an appropriate Docker image

- https://hub.docker.com/

## 26. Lightweight Docker images (alpine, slim)

Every docker image should be downloaded. So always need to find lighter images

choose image: image: node:22-alpine(the lightest) or node:22-slim(lighter) instead of image: node:22
Time of job execution has been reduced in TWICE (from 30sec to 15sec) after usage of node:22-alpine instead of node:22

## 27. Building the project using GitLab

Use `npm ci` instead of `npm install`. It's optimized for ci and uses dependencies from `package-lock.json`, but not `package.json`.

stages:

- build

test_npm:
image: node:22-alpine
stage: build
script: - node --version - npm --version - npm ci - npm run build

## 28. Assignment: Publishing build artifacts

- https://docs.gitlab.com/ci/jobs/job_artifacts/
- https://docs.gitlab.com/ci/yaml/#artifacts

## 29. Assignment: Publishing build artifacts

stages:

- build

test_npm:
image: node:22-alpine
stage: build
script: - node --version - npm --version - npm ci - npm run build
artifacts:
paths: - build/

## 30. Revisiting the GitLab CI/CD architecture

1.  GitLab Server find GitLab Runner and give its instructions
2.  Executing job on the runner in Docker container
3.  Save artifacts on GitLab Server

One docker container is used ONLY for one JOB

## 31. Assignment: Adding a test stage

## 32. Assignment: Adding a test stage - Solution

stages:

- build
- test

build_website:
....

test_artifact:
image: alpine
stage: test
script: - echo "Testing" - test -f "build/index.html"

## 33. Running unit tests

stages:

- build
- test
- unit

...

unit_tests:
image: node:22-alpine
stage: unit
script: - npm ci - npm test

## 34. Running jobs in parallel

stages:

- build
- test

...

test_artifact:
image: alpine
...

unit_tests:
image: node:22-alpine
stage: test
...

## 35. Default pipeline stages (.pre, build, test, deploy .post)

GitLab define next stages by default and you can omit `stages` declaration:

stages:

- .pre
- build
- test
- deploy
- .post

## 36. Publishing a JUnit test report

- https://docs.gitlab.com/ci/yaml/#artifactsreports
- https://docs.gitlab.com/ci/testing/unit_test_reports/
- https://docs.gitlab.com/ci/testing/unit_test_report_examples/

...
unit_tests:
image: node:22-alpine
stage: test
script: - npm ci - npm test
artifacts:
when: always
reports:
junit: reports/junit.xml

## 37. Testing the tests (ensure that the tests fail!)

## 38. How to integrate changes?

- https://sisense.udemy.com/course/gitlab-ci-pipelines-ci-cd-and-devops-for-beginners/learn/lecture/48223617#overview

## 39. Configuring Merge Requests

Fast-Forward merge, encourage squash, Protect branch.

## 40. Making changes through a Merge Request

- https://gitlab.com/forelock/learn-gitlab-app/-/merge_requests/1

## 41. Code review and merging changes

- https://gitlab.com/forelock/learn-gitlab-app/-/merge_requests/1

## 42. Configuring a code linter

- https://docs.gitlab.com/ci/testing/code_quality/#eslint
- https://www.npmjs.com/package/eslint-formatter-gitlab
  Copy code of stage:
  ...
  eslint:
  image: node:20-alpine
  script: - npm ci - npx eslint --format gitlab .
  artifacts:
  reports:
  codequality: gl-codequality.json --- new widget in GitLab MR
  ...

## 43. How to structure a pipeline?

If there is quick/fast stages, like eslint put it as soon a possible to reduce time execution if fails

## 44. Simplifying the pipeline

Put `.` to disable stage execution

.test_artifact:
image: alpine
stage: test
script: - echo "Testing" - test -f "build/index.html"

You can override stage just define it with the same name after.
