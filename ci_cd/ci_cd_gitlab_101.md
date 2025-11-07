# CI/CD on GitLab 101

This guide is a basic introduction of CI/CD implementation on private _GitLab_
server.

## Prerequisites

- private _GitLab_ server;
- access credentials to the _GitLab_ server;
- configured project (refer to _poetry 101_ guide);

## Configure .gitlab-ci.yml

`.gitlab-ci.yml` file responsible for the pipeline of CI/CD on your
GitLab server, most of configurations are located here.

```yml
default:
  image: python:3.12
  before_script:
    - pip install poetry
    - poetry install
stages:
  - test
  - build
test:
  stage: test
  script:
    - echo "This is the test stage"
    - poetry run pytest --junitxml=tests/report.xml -v
  artifacts:
    when: always
    reports:
      junit: tests/report.xml
building:
  stage: build
  needs: [test]
  script:
    - echo "This is the build stage"
    - poetry config repositories.gitlab http://gitlab.bitrobotics.com/api/v4/projects/${CI_PROJECT_ID}/packages/pypi
    - echo "Repository gitlab configured ..."
    - poetry build
    - echo "Build done ..."
    - poetry config http-basic.gitlab gitlab-ci-token ${CI_JOB_TOKEN}
    - poetry publish --repository gitlab
    - echo "Publishing done!"
```

- `image` - image name of the docker container that will run your jobs,
name have to match docker container;
- `stages` - here you specify stages of your pipeline, each stage will
have a separate container initialized (in this example there `test` and
`build`, `publishing` is included into the `build` and `deploy` is not
necessary in our context);
- `poetry run pytest --junitxml=tests/report.xml -v` - tells pytest to
generate `report.xml` (add it to `.gitignore`), which is a standardized
GitLab report, you can collect metrics on your tests later on GitLab;
- `artifacts` - tells GitLab to collect those reports;
- `needs` with `[test]` - specifies requirements for the stage to start,
here `building` stage will not start without successful `test` stage;
- `poetry config repositories.gitlab <url>` - is where GitLab registry
located and your built package will be placed (more about it in _package publishing).

## GitLab runner

GitLab runner is a docker that will run your test, build or publish job,
one at a time. This is just one of the options to implement CI/CD and
requires the least amount of configuration. Runners can be installed on the same server where GitLab is installed or else where.

> Make sure that both GitLab and Runners have full set of permissions
to run jobs.

### Create GitLab runner

If your are making a new pipeline it most likely that you do not have a
runner for it yet and you have to create one.

1. Push your project to the GitLab with configured `.gitlab-ci.yml`, you
will see a new pipeline on the page of the project;
2. Click on the pipeline and GitLab will suggest you to create a new
runner;
3. Copy suggested code, log into server with GitLab runners installed and
pass suggested code (make sure to create an image with the name specified
in the yml file);
4. When runner will be created, rerun pipeline.