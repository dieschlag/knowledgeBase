Steps:
- activate runners on the project
- create a `.gitlab-ci.yaml` file at the root of repository. This is where we define the Ci/CD job.

If a runner is not installed on our machine, we can do so by following this installation process: https://docs.gitlab.com/runner/install/.

We also need to register the runner (TO CHECK).

For the `.gitlab-ci.yaml` file: it is a yaml file in which we specify 
- the structure and order of jobs that the runner should execute
- the decisions the runner should make when specific conditions are met.

Here is an example from the official documentation:

```yaml
build-job:
  stage: build
  script:
    - echo "Hello, $GITLAB_USER_LOGIN!"

test-job1:
  stage: test
  script:
    - echo "This job tests something"

test-job2:
  stage: test
  script:
    - echo "This job tests something, but takes more time than test-job1."
    - echo "After the echo commands complete, it runs the sleep command for 20 seconds"
    - echo "which simulates a test that runs 20 seconds longer than test-job1"
    - sleep 20

deploy-prod:
  stage: deploy
  script:
    - echo "This job deploys something from the $CI_COMMIT_BRANCH branch."
  environment: production
```

Here we have four jobs:
- build-job
- test-job1
- test-job2
- deploy-prod

The echo commands will display text in the UI when a job is executed. The other vars like `$GITLAB_USER_LOGINis` are populated when the job runs.

We can view the status of a pipeline in `Build > Pipelines`.

Each job has a script section and belongs to a stage. `stage` describes the order of execution. Jobs that are part of the same stage run in parallel.

The `needs` keyword can be used to run jobs out of stage order and therefore increase pipeline speed. Here is an example:

```yaml
linux:build:
  stage: build
  script: echo "Building linux..."

lint:
  stage: test
  needs: []
  script: echo "Linting..."

linux:rspec:
  stage: test
  needs: ["linux:build"]
  script: echo "Running rspec on linux..."
```

The `lint` job starts immediately, while the `linux:rspec` only starts once the `linux:build` has finished.

A few extra keywords:

- `rules` keyword is used to specify when to run or skip jobs. Here is an example:

```yaml
job:
  script: echo "Hello, Rules!"
  rules:
    - if: $CI_MERGE_REQUEST_SOURCE_BRANCH_NAME =~ /^feature/ && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME != $CI_DEFAULT_BRANCH
      when: never
    - if: $CI_MERGE_REQUEST_SOURCE_BRANCH_NAME =~ /^feature/
      when: manual
      allow_failure: true
    - if: $CI_MERGE_REQUEST_SOURCE_BRANCH_NAME
```

Ici, on choisit d'exécuter le job uniquement si certains critères sont rencontrés dans le nom de la branche source. 

Le `when: nerver` signifie que le job ne s'executera jamais lorsque la condition est rencontré.

Le `when:manual` signifie que le job peut s'exécuter que depuis l'interface GitLab, manuellement.

Pour le dernier `if`, le `when` par défaut s'applique, qui est en fait un `when: on_success` is used, meaning that the job is executed if the other rules are not matched. It serves as a default rule.

Before the `rules` keyword was introduced, the `only` and `except` keywords where used, and allowed (or restricted) the start of the job if a given or a set of conditions were met.

Here is an example on a job only applying on the `master` or on the `merge_requests` branches:

```yaml
deploy:
  only:
    - master
    - merge_requests
  extends: .serverless:deploy:image
  environment:
    name: ${CI_COMMIT_REF_SLUG}
    url: http://${CI_PROJECT_NAME}.${CI_PROJECT_NAME}-${CI_PROJECT_ID}-${CI_COMMIT_REF_SLUG}.${KUBE_INGRESS_BASE_DOMAIN}
    on_stop: stop-review
  # overriding of the script to pass environment variable(s)
  script: /usr/bin/gitlabktl app deploy --env "TARGET:Wonderful World"
```

- `cache` keyword can be used to specify a list of files and directories we want to be cached between jobs. We can only use paths that are in the local working copy.

Caches are:
- shared between pipelines and jobs
- not shared between protected and unprotected branches
- restored before artifacts
- limited to a maximum of different caches

Example of a cache:

```yaml
rspec:
  script:
    - echo "This job uses a cache."
  cache:
    key: binaries-cache
    paths:
      - binaries/*.apk
      - .config
```


- `artifacts` keyword to specify which files to save as job artifacts. These artifacts are a list of files and directories that are attached to the job when it succeeds/fails/always.

These files are available for download after the job finishes if the size is smaller than the maximum artifact size.

By default jobs in later stages automatically download all artifacts created in earlier stages.

Jobs defined with the `needs` keyword can only download files artifacts from the jobs they depend on.

These artifacts are collected only for successful jobs by default and are restored after caches.

Here is an example of an artifact:

```yaml
artifacts:
  paths:
    - binaries/
  exclude:
    - binaries/**/*.o
```

- default vars can be defined using `default` keyword. These defaults vars are copied for all jobs if they don't have a given keyword already defined.

Here is an example of a default:

```yaml
default:
  image: ruby:3.0
  retry: 2

rspec:
  script: bundle exec rspec

rspec 2.7:
  image: ruby:2.7
  script: bundle exec rspec
```

The default is particularly useful to define `before_script` and `after_script` actions. As a reference, here is an example of a before_script:

```yaml
job:
  before_script:
    - echo "Execute this command before any 'script:' commands."
  script:
    - echo "This command executes after the job's 'before_script' commands."
```

- `include` keyword: is used to include another .yaml file in the CI/CD config, and allows to split our CI/CD in several files.

These included files are always first evaluated before being merged with the content of the gitlab-ci.yaml file.

