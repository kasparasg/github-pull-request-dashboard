#github-pull-request-dashboard

A simple web application which shows outstanding pull requests for repos it has been configured to watch. Shows the following information for each PR:

* Whether or not it is assigned (and to whom).
* How long the PR has been open for.
* Who opened the PR.

PRs are sorted in to 'assigned' and 'unassigned' groups and are oldest first within each group in order to encourage teams to work in a FIFO manner and not to pick favourites.

Each piece of PR information contains a link to the PR in GitHub.

#Origins

This project started life as an internal tool in [HMRC](https://github.com/hmrc)'s WebOps team, the initial development was done mostly by [Stephen Crossan](https://github.com/scrossan).
WebOps' Digital Service Manager [Kalbir Sohi](https://github.com/kalbir) and Steve Crossan have both kindly agreed that this 'generified' code can now be open sourced.

Open the project in your favourite IDE, don't forget to point the IDE at the virtualenv (see path above)

# Running the app

## Pre-requisites

We use [pyenv](https://github.com/yyuu/pyenv) and [pyenv-virtualenv](https://github.com/yyuu/pyenv-virtualenv) to manage the Python installation and the VirtualEnv respectively. The `makefile` will not work without these.

### Environment prep

Create and activate a virtual environment by running the following comands in the root of this directory:

    make venv
    source ~/.pyenv/versions/venv-github-pull-requests-dashboard-2.7.10/bin/activate

Install the required dependencies in to the VirtualEnv:

    make pip

## Configuration

Before the app can be run it will need some basic configuration:

### Create an Oauth Token

The application authenticates against GitHub using an OAuth token, [details here](https://github.com/blog/1509-personal-api-tokens) on how to create one:

Your OAuth token requires the following permissions:

* repo
* public_repo
* repo:status

### Configure the repos you want to watch

Watched repos are expressed in a YAML document which is passed to the application via an environment variable named `REPOS`. The configuration structure is made up of a list of watched 'groups'. Each group is given a name and one or more repositories to watch.

Here's an example of the required YAML document structure.

    ---
    -
      group: edd
      description: PRs on Edd's Projects
      watches:
        -
          owner: eddgrant
          repo: vagrant-roller
        -
          owner: eddgrant
          repo: github-pull-request-dashboard
    -
      group: beeb
      watches:
        -
          owner: bbc
          repo: gatling-load-tests
        -
          owner: bbc
          repo: hive-runner
    -
      group: hmrc
      watches:
        -
          owner: hmrc
          repo: ct-calculations


The app can be run either directly from source or as a docker container, the configuration in both cases is the same:

### Running from source

`run.sh` provides an example of how to run the application from source, simply configure the file with your OAuth token and the repositories you want to watch and then execute the script.

The application will then start and will be available at http://localhost:8000

### Running as a docker container

This is a work in progress and will be detailed soon.
    

