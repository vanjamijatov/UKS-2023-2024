# Software Configuration Management - A Master Course for Exchange Students

This repository contains materials and guidelines intended to be used by exchange students enrolled 
at the Software Configuration Management course. Only practical classes are covered by this document. 


## The project specification 

To pass the practical part of the course, student is obliged to conduct a project as a member of a team.
The project is focused on 
properly maintaining [the web-application (simplified *GitHub*)](https://docs.google.com/document/d/1iekw6HSwoa1iIgp2G5sW72qdfjm1mXq9g99kkge1nEo/edit?usp=drive_link) life cycle from its early beginning until deploying in production.
The total number of points that can be achieved by successfully finishing the project is 60. 
The following list covers the items that form the project and the points that can be 
earned by properly conducting each of them:
- (6 points) A Class Diagram that describes the model of you application.
- (20 points) Implementation of [the web application (simplified *GitHub*)](https://docs.google.com/document/d/1iekw6HSwoa1iIgp2G5sW72qdfjm1mXq9g99kkge1nEo/edit?usp=drive_link) 
- (6 points) Implementation of the tests:
  - (2 points) Unit tests - Each component (a class) should be covered by unit tests.
  - (4 points) Integration tests - The server side of the application should be covered by integration tests.
    Mock up the client which sends the HTTP requests.
- (10 points) Git Tool Usage - The git tool is intended to be used by respecting the [GitFlow Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow). 
  More information can be found in the corresponding section below.
- (10 points) GitHub Platform Usage - To track down the project development, GitHub platform should be used. 
  More information can be found in the corresponding section below.
- (8 points) Software Configuration Management: 
  - (1 points) The application should be containerized in the corresponding image. 
  - (3 points) Container Orchestration - It is expected to orchestrate the following containers (more containers can be introduced in the system):
    - app container - The web-application runs inside this container.
    - db container - The web-application is supposed to store the date in the database running in dedicated container.
    - reverse proxy container - The reverse proxy should be placed between the user and the application. It runs in separate container.
    - mem cache container - The application should used in-memory cache to be more responsive. 
  - (4 points) Usage of CI/CD tools - From the early beginning of the application development, students should use use a CI/CD Tool.
    It is expected to provide the following build plans:
    - (1 point) A build plan triggered when pull request is created on the *develop* branch. This build plan is responsible to run all automated tests and provide the output 
      whether it is safe to merge the pull request.
    - (3 points) A build plan triggered when new version of the app is pushed on the main/master branch.
      This build plan runs all automated tests. If they pass, the application should be containerize and the corresponding image is pushed
      on public registry.
      
## Python Programming Languages

It is recommended to use Python programming language. The guidelines about the basic usage of Python programming language
and its tools can be found [at this repository](https://github.com/vanjamijatov/UKS-materijali/tree/main/Python%20Basics/Python%20lesson).

## Django Web Framework
It is recommended to use Django Web Framework for the application development. 
A good starting point to introduce yourself to the framework is by doing
the [following tutorial](https://github.com/vanjamijatov/UKS-materijali/tree/main/Django%20Framework). 
It covers pretty much all basic stuff needed for this project. 

A more complex Django example can be found [here](https://github.com/vanjamijatov/UKS-DjangoAuthTestsDocker).
It is possible that names and comments found here are written in Serbian. 
If this represents the problem, the [Google Translate](https://translate.google.com/) can be used.
It works pretty well for Serbian to English translations (and vice versa).
This is a nice way to learn some Serbian words which may ease the life in Novi Sad. 

### Auth System
The Django supports built-in authentication and authorization system. 
The example of how to use it can be found [at this repository](https://github.com/vanjamijatov/UKS-DjangoAuthTestsDocker):
- The permission, groups and users can be [created dynamically](https://github.com/vanjamijatov/UKS-DjangoAuthTestsDocker/blob/master/prodavnicesajt/prodavnice/management/commands/popuni_bazu.py) .
- The permission of whether an user is allowed to do something can be [set on Django view function](https://github.com/vanjamijatov/UKS-DjangoAuthTestsDocker/blob/master/prodavnicesajt/prodavnice/kasa_view.py). 
  - See `@login_required` and `@permission_required` decorators.
- The login and log out can be [done like this](https://github.com/vanjamijatov/UKS-DjangoAuthTestsDocker/blob/master/prodavnicesajt/prodavnice/kasa_view.py).
  - Check `kase_login` and `kase_logout` functions ("Kase" means cash desks, "Prodavnice" means Shops, etc. in Serbian)
    (FIXME: I guess we could fix to use Kase instead of prodavnice in the example)

### Django Testing
Django supports [automated testing](https://docs.djangoproject.com/en/4.1/topics/testing/).
The test suite is a class that extends `unittest.TestCase` class. 
A methods that starts with `test_` prefix represent an individual test case.
The test suites are written inside Python modules named with `test` prefix. They're usually placed
inside `tests` folder. 

#### Django Unit Testing
[Unit tests](https://en.wikipedia.org/wiki/Unit_testing) are supposed to test one component in isolation (e.g. behaviour of one class). 
The example of writing a unit test can be found [here](https://docs.djangoproject.com/en/4.1/topics/testing/overview/#writing-tests)

#### Django Integration Testing
[Integration testing](https://en.wikipedia.org/wiki/Integration_testing) is used to test whether the integration among multiple components works fine.
For the purpose of this project, you're expected to write this kind of test to validate the functionalities of the server side of the application.
The example that should use as a reference while writing this kind of tests is class [`KaseLoginTests`](https://github.com/vanjamijatov/UKS-DjangoAuthTestsDocker/blob/master/prodavnicesajt/prodavnice/tests/test_kase_login.py).
It represents the test suite which validates whether the login and logout is well implemented. 
It is important to pay attention to the following:
- To initialize the database for the test suite, `setUpTestData` class method can be used.
- To do the set up before each test case, `setUp` instance method can be used. 
- To test only the server side of the application, the client should be mocked by using the 
  [`Client` test class](https://docs.djangoproject.com/en/4.1/topics/testing/tools/#the-test-client).
- To send HTTP requests, the corresponding `Client` methods ca be used (`get, post, put, delete`).
- The `TestCase` class has a set of assertion methods used to validate the response.

To run the `KaseLoginTests`, the following command can be used:
```
python manage.py test prodavnice.tests --noinput
```
Note that after running this suite of test, the database is deleted. 
To change this, [--noiput arugment should be replaced](https://docs.djangoproject.com/en/4.1/topics/testing/overview/#the-test-database).


#### Runnin Django tests
Different ways to run Django tests can be found [here](https://docs.djangoproject.com/en/4.1/topics/testing/overview/#running-tests).

## Git
The mandatory version control system planned to be used while developing the project is [git](https://git-scm.com/).
It is expected to be used by respecting the [GitFlow Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow).
The *main/master* branch stores the official release history. For this project, you will publish 2-3 versions that should correspond
to the check points we will schedule during semester. 
It's also convenient to tag all commits in the *main/master* branch with a version number.

The *develop* branch serves as an integration branch for features. You will frequently submitting the code to this branch.
The code present on this branch should worked and be fully tested. When new feature named as *<feature_name>* is planned to be developed,
the first step is to create a new *feature-<feature_name>* branch by checking out from the latest commit of *develop* branch.
When the feature is implemented, it should be tested on the feature branch. After that, the feature branch should be integrated
with the *develop* branch through the corresponding *pull request* responsible to trigger the build plan that runs all tests 
(see above about the build plans that should be supported). If the build plan succeeds, then the pull request can be merged to the *develop* branch.

Similarly, if you detect a bug on the develop branch, the first step is to checkout from the latest commit of that branch
and create the *bugfix-<feature_name>* branch, where *<feature_name>* is the name of feature which induced the bug.
After the bug is fixed and the fix is tested, the code should be integrated on *develop* branch by corresponding pull request.

**Note:** Each feature or bugfix branch should correspond to exactly one GitHub issue. See below about the way GitHub issues should be maintained.

## GitHub 
Beside hosting your git repository, this platform is used to track down the working progress. 
To do this, milestones should be created, which number should match the number of control check points. 
You can name them as "Check Point 1", "Check Point 2", "Final Submission". If you would like to be more
creative, feel free to do that. The purpose of each check point is to describe the phase in the 
life cycle of the application. 

When do the planning, plan always for the most recent milestone. To do this, try to see what features are you 
going to develop until the milestone's deadline. Each feature should be represented by a GitHub issue (task)
that is connected to the milestone. Before starting to develop a feature, each team member should assign the feature
to himself/herself of to some other team member. After the assignment is done, the corresponding feature breanch 
should be created and the development is done as described in the previous section. After the feature is developed
and tested, the corresponding issue should be closed. 

In case you detect the bug, you should reopen the previously closed issues, rather than creating a new one. 

Before the specified deadline, each milestone should be 100% finished. The progress of each milestone is
automatically tracked by the GitHub platform.

## GitHub Actions
A CI/CD (Continuous Integration/Continuous Delivery) tool recommended to be used is [GitHub Actions](https://github.com/features/actions).
Its usage should be free of charge for open source projects (public repositories), but it would be good to consult their 
[pricing plan](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions). 
In the context of GitHub Actions, build plans are called as workflows. A workflow can contain a set of jobs triggered when the corresponding 
event occurred. The worfklow is executed on the specified runner (a VM present somewhere in the cloud).
The results of all workflows can be found in [Actions tab](https://github.com/vladaindjic/DjangoAuthTests/actions).

A simple workflow can be found in this [django.yml file](https://github.com/vanjamijatov/UKS-DjangoAuthTestsDocker/blob/master/.github/workflows/django.yml). 
As one can notice, this workflow is triggered when pull request is created on *master* branch or if the code has been pushed to the same branch.
This workflow requests the runner with the latest Ubuntu distribution. The runner then checkouts the latest version of the repository, set ups the python-3.9 intepreter,
installs python packages specified in the `requirements.txt`. After that, the Django database is prepared, followed by the command that runs the 
specified test suites. 

Note that workflows should be placed in `<repo_root_dir>/.github/workflows/ directory`. 

For more information aboud what each YAML element represents, you may consult the 
[documentation](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions#understanding-the-workflow-file)

From the early stages of the application development, you should use the tool for continuous integration. 

## Docker
A tool for the containerization recommended to be used is the Docker. 
To use it, [Docker Engine](https://docs.docker.com/engine/install/) needs to be installed first.

### Theory behind containers 
Despite the VMs that use hardware virtualization, containers utilizes OS "virtualization"
(see [the following post](https://www.netapp.com/blog/containers-vs-vms/) for more information).

[Containers](https://www.ibm.com/cloud/learn/containers) represent executable units
of software in which application code is packaged, along with its libraries and dependencies, 
in common ways so that it can be run anywhere, whether it be on desktop, traditional IT, or the cloud. 
To do this, containers take advantage of a form  of operating system (OS) virtualization in which features 
of the OS (in the case of the Linux kernel, namely the namespaces and cgroups primitives) are leveraged to both 
isolate processes and control the amount of CPU, memory, and disk that those processes have access to.
Containers are small, fast, and portable because unlike a virtual machine, containers do not need include a 
guest OS in every instance and can, instead, simply leverage the features and resources of the host OS.

Even though containers running on the same host OS may simulate different Linux distributions
[specified by the corresponding base image](https://serverfault.com/questions/755607/why-do-we-use-a-os-base-image-with-docker-if-containers-have-no-guest-os)),
they all share the same host's kernel. A container is isolated from the rest of the Linux processes
by leveraging the Linux's *cgroups* and *namespaces* technologies. If you're interested to get more insights about
what happens under the hood, you may read the [following blog posts](https://www.nginx.com/blog/what-are-namespaces-cgroups-how-do-they-work/?fbclid=IwAR1AZLm53SCu9_4oYW9x6jEs1iOgLNixgA-p74Qgq3KVcb8Rn87xmoNWkzM).

### Creating a Docker Image 
The docker image represents the schematic description of the docker container and its runtime environment.
The image can be considered as a template for creating the container. 

The convenient way of creating a docker image is by specifying the corresponding [`Dockerfile`](https://github.com/vanjamijatov/UKS-DjangoAuthTestsDocker/blob/master/Dockerfile).
As one may notice, the first line represents the base image used for building this one. 
The commands in the following line are adding layers to the base image. The more layer image has, the bigger it is. 
The description of each command can be found in the comment above. 

More information about the `Dockerfile` commands can be found in [the official documentation](https://docs.docker.com/engine/reference/builder/).

After the `Dockerfile` is specified, the image is [ready to be built](https://github.com/vanjamijatov/UKS-DjangoAuthTestsDocker#run-from-local-build).


### Running a Docker Container
To run the container, the corresponding image must be specified.
Container can be run by using the [image built locally](https://github.com/vanjamijatov/UKS-DjangoAuthTestsDocker#run-from-local-build)
or the [image present at the public image repository](https://github.com/vanjamijatov/UKS-DjangoAuthTestsDocker#run-from-docker-hub). 


## Docker Hub
A public image registry/repository recommended to be used is Docker Hub.
When the image is built locally, it can be shared with other users by uploading it to the Docker Hub.
To do that, follow [the guidelines present here](https://github.com/vanjamijatov/UKS-DjangoAuthTestsDocker#upload-the-local-image-to-docker-hub). 


## Docker Compose
A tool for orchestrating multiple docker containers on a single machine recommended to be used is Docker Compose. 
To install the Docker Compose, follow [these instructions](https://docs.docker.com/compose/install/).

To orchestrate multiple docker containers on a single host, it is enough to provide [docker-compose.yml file](https://github.com/vanjamijatov/UKS-DjangoProductionSetup/blob/master/docker-compose.yml)
This configuration file specifies 4 different service (containers):
- db - The PostgreSQL runs inside this container.
- web - The Django application runs inside this container.
- nginx - NGINX reverse proxy server runs in this container
- redis - Redis in-memory key-value database runs in this container and serves as a mem-cache for Django application.

When specifying a container/service in configuration one, one may specified following elements:
- `container_name` - specifies container name
- `image` - image used to run the container
- `ports` - specified the mappings between host ports and container ports
- `environment` - specifies the set of environment variables used while container is running
- `build: .` - specifies that the container is built from local image whose `Dockerfile` can be found in current directory
- `command` - what command to execute when starting a docker container. Note that `web` container runs the [wait_for_postgres.sh script](https://github.com/vanjamijatov/UKS-DjangoProductionSetup/blob/master/scripts/wait_for_postgres.sh) 
  when starting the container. This script waits for `db` container to start (PostgreSQL database should start). After that,
  the [start-django.sh script](https://github.com/vanjamijatov/UKS-DjangoProductionSetup/blob/master/scripts/start-django.sh)
  is run and is responsible to prepare database and run Django application inside gunicorn server.
- `expose` - expose the port of the container to be seen by other containers in the network. Note that no mapping to
  the host ports is specified.
- `depends_on` - specifies the dependencies among containers
- `links` - specifies aliases that can be used to reference this container from other docker compose containers.

The container can use persistent storage, which content is available after the container stops running. 
In this example, [bind mounts](https://docs.docker.com/storage/bind-mounts/) are used and specified by `volumes` element. 
This kind of storage simply mounts specified host file system directory subtree to a certain location in container file system. 
Another kind of persistent storage that can be used by the container is [volume](https://docs.docker.com/storage/volumes/).
This is what official documentation says about the difference between a bind mount and a volume:
*"Bind mounts have limited functionality compared to volumes. When you use a bind mount, a file or directory on 
the host machine is mounted into a container. The file or directory is referenced by its
absolute path on the host machine. By contrast, when you use a volume, a new directory is 
created within Docker’s storage directory on the host machine, and Docker manages that 
directory’s contents."*. See [documentation](https://docs.docker.com/storage/bind-mounts/) for more information.

If no persistent storage is specified, then the container stores the data in writable layer,
which is destroyed after container stops. To preserve data between multiple runs of the same container,
it is advised to used persistent storage. 

By default, docker compose creates a default bridge network and connects all specified container to it. 
The created network is isolated from the rest of the system. The DNS resolution withing the network
is inherited from the Linux host. For more information about the network management, read the
following [blog post](https://earthly.dev/blog/docker-networking/).

### Django configuration for Docker Compose
Since the Django application should use the PostgreSQL instead of the default SQLite3,
the [`DATABASES` global variable should be adapted](https://github.com/vanjamijatov/UKS-DjangoProductionSetup/blob/master/prodavnicesajt/prodavnicesajt/settings.py#L74)

To use the Redis as a in memory cache, the [CACHES global variable](https://github.com/vanjamijatov/UKS-DjangoProductionSetup/blob/master/prodavnicesajt/prodavnicesajt/settings.py#L158)
should be introduced. 

### CI/CD and Docker Compose
For this project, it is enough to run tests locally on the GitHub Actions runner.
There is no need to test the integration among all 4 containers. 
It is enough to use the default SQLite 3 database while running Django integration tests. 

In [the provided example](https://github.com/vanjamijatov/UKS-DjangoProductionSetup) integration tests runs
in the test environment which is represented by the Django application and the default SQLite database.
The production environment is represented by the docker compose configuration file which specifies the 
docker containers described above. The production environment assume usage of real PostgreSQL database
server and the in-memory key-value redis database. 

The [Django settings.py module](https://github.com/vanjamijatov/UKS-DjangoProductionSetup/blob/master/prodavnicesajt/prodavnicesajt/settings.py)
should be able to dynamically choose the setup which corresponds to either test or production environment.
The module assumes that the Linux environment variable [UKS_TEST_DB](https://github.com/vanjamijatov/UKS-DjangoProductionSetup/blob/master/prodavnicesajt/prodavnicesajt/settings.py#L143) 
specifies the type of environment. If the value of the variable is `ON`, then the test environment is
active and the `DATABASES` variable is set the use the SQLite database. Otherwise, PostgreSQL an Redis
is used. Note that [when triggering the workflow's jobs](https://github.com/vanjamijatov/UKS-DjangoProductionSetup/blob/master/.github/workflows/django.yml#L10),
the `UKS_TEST_DB` variable is set to `ON`, which
means that the runner setups the test environment and the Django application uses the SQLite3 database.


## Important Notes 
- **Students should form teams and conduct a project***.
- Number of students that form one team depends on the total number of enrolled students in this course.
- This is the early version of the document and can be changed over time. 
- More information about the application's features and functionalities will be provided soon. 
- If you notice any inconvenience, feel free to open an issue or create a pull request with the fix. 
