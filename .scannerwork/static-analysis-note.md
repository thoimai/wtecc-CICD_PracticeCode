

# Step 1: Setup PostgresSQL database 
`docker network create mynet`
`docker run --name postgres  -e POSTGRES_USER=root -e POSTGRES_PASSWORD=Test12345  -p 5432:5432 --network mynet -d postgres`


# Step 2: Setup SonarQube server 

# Task 
```shell
docker run -d --name sonarqube -p 9000:9000 -e sonar.jdbc.url=jdbc:postgresql://postgres/postgres -e sonar.jdbc.username=root -e sonar.jdbc.password=Test12345 --network mynet sonarqube
```

## Check for success 
```log
theia@theiadocker-u1513319:/home/project$ docker ps -a 
CONTAINER ID   IMAGE       COMMAND                  CREATED         STATUS         PORTS                                       NAMES
8e59bdeaa531   sonarqube   "/opt/sonarqube/dock…"   2 minutes ago   Up 2 minutes   0.0.0.0:9000->9000/tcp, :::9000->9000/tcp   sonarqube
53a571fcc386   postgres    "docker-entrypoint.s…"   7 minutes ago   Up 7 minutes   0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   postgres
```

# Step 4: Create a SonarQube Project 

> Generate project token 

`sqp_9667e5b7e9251205d71cb8c3051f93118c5e25b7`

> Run SonarQube Analysis

```shell
sonar-scanner \
  -Dsonar.projectKey=temp \
  -Dsonar.sources=. \
  -Dsonar.host.url=https://u1513319-9000.theiadocker-3-labs-prod-theiak8s-4-tor01.proxy.cognitiveclass.ai \
  -Dsonar.token=sqp_9667e5b7e9251205d71cb8c3051f93118c5e25b7
```


# Step 6: Ready the SonarQube Scanner 

It is important to understand that the SonarQube server that stores the results of scans is separate and distinct from the SonarQube scanner which performs the actual scanning. Up until now, we’ve created a database for storing the analysis results and provisioned a SonarQube server for serving the UI.

To get the SonarQube scanner to work in the Cloud IDE, you can either install it locally or pull its docker image and run its docker container. In this lab, you will be pulling the docker image and running its docker container.

1. First, we will use the docker pull command to download the sonarsource/sonar-scanner-cli image from Docker hub so that it is available locally for use.

`docker pull sonarsource/sonar-scanner-cli`

2. Run the following bash `alias` command in the terminal, which creates an alias `sonar-scanner` for running the scanner later using the `scanner-cli` docker container:

`alias sonar-scanner='docker run --rm -v "$(pwd):/usr/src" sonarsource/sonar-scanner-cli'`

    > Note: This command is mounting the current working directory as a volume at /usr/src inside the container, which is where sonar-scanner is looking for the source code. You can set this up on your own computer as well.

> cmd explain 
The command you've provided creates an alias named `sonar-scanner` that runs a Docker command to start a SonarScanner CLI (Command Line Interface) container. Here's a breakdown of what each part of the command does:

`alias sonar-scanner=`

This creates an alias named `sonar-scanner`. After this command is run, you can use `sonar-scanner` in your terminal to execute the Docker command that follows.

`'docker run --rm -v "$(pwd):/usr/src" sonarsource/sonar-scanner-cli'`

This is the Docker command that will be run when you use the `sonar-scanner` alias. Here's what each part of this command does:

- `docker run`: This starts a new Docker container.
- `--rm`: This flag tells Docker to automatically remove the container when it exits. This is useful for temporary tasks, like running a SonarScanner analysis, to avoid leaving unused containers lying around.
- `-v "$(pwd):/usr/src"`: This flag mounts the current directory (`$(pwd)`) on the host machine to the `/usr/src` directory in the Docker container. This allows the SonarScanner container to access the code in the current directory.
- `sonarsource/sonar-scanner-cli`: This is the Docker image that the container will be based on. If this image isn't already available locally, Docker will download it from Docker Hub.

So, in summary, after running this command, you can use `sonar-scanner` in your terminal to run a SonarScanner analysis on the code in the current directory. The analysis results will be sent to the SonarQube server that the SonarScanner CLI is configured to use.

> explain the mount process 
Here's a visualization of the mount process from the command you provided:

![Mount Process](https://showme.redstarplugin.com/d/jW72Oe6V)

In this diagram:

- The `Host Machine` represents your local machine where you're running the Docker command.
- The `Docker Daemon` is the background service running on the host that manages building, running and distributing Docker containers.
- The `SonarScanner Container` is the Docker container that is being run.
- The `Volume` represents the current directory on your host machine. This is determined by the `pwd` command.

When you run the Docker command with the `-v "$(pwd):/usr/src"` option, the Docker daemon creates a mount point with the path from the `pwd` command (which represents the current directory on your host machine). This directory is then mounted as `/usr/src` in the `SonarScanner Container`. This means that the SonarScanner container can access and use the files in the current directory of your host machine.

[You can view this diagram in a new tab.](https://showme.redstarplugin.com/d/jW72Oe6V)

[You can edit this diagram online if you want to make any changes.](https://showme.redstarplugin.com/s/txvpf6PZ)

The type of the diagram is a graph in Mermaid language.

To view ideas for improving the diagram, use the key phrase "*show ideas*".

To view other types of diagram and languages, use the key phrase "*explore diagrams*".


# Step 8: Run the Scanner 

> Static Analysis Scanning results 

```log 
theia@theiadocker-u1513319:/home/project/wtecc-CICD_PracticeCode$ sonar-scanner \
>   -Dsonar.projectKey=temp \
>   -Dsonar.sources=. \
>   -Dsonar.host.url=https://u1513319-9000.theiadocker-3-labs-prod-theiak8s-4-tor01.proxy.cognitiveclass.ai \
>   -Dsonar.token=sqp_9667e5b7e9251205d71cb8c3051f93118c5e25b7
INFO: Scanner configuration file: /opt/sonar-scanner/conf/sonar-scanner.properties
INFO: Project root configuration file: NONE
INFO: SonarScanner 5.0.0.2966
INFO: Java 17.0.7 Alpine (64-bit)
INFO: Linux 5.4.0-153-generic amd64
INFO: User cache: /opt/sonar-scanner/.sonar/cache
INFO: Analyzing on SonarQube server 10.1.0.73491
INFO: Default locale: "en_US", source code encoding: "UTF-8" (analysis is platform dependent)
INFO: Load global settings
INFO: Load global settings (done) | time=219ms
INFO: Server id: 147B411E-AYmrzK_jFf_9kO6CA8RN
INFO: User cache: /opt/sonar-scanner/.sonar/cache
INFO: Load/download plugins
INFO: Load plugins index
INFO: Load plugins index (done) | time=113ms
INFO: Load/download plugins (done) | time=20610ms
INFO: Process project properties
INFO: Process project properties (done) | time=11ms
INFO: Execute project builders
INFO: Execute project builders (done) | time=2ms
INFO: Project key: temp
INFO: Base dir: /usr/src
INFO: Working dir: /usr/src/.scannerwork
INFO: Load project settings for component key: 'temp'
INFO: Load project settings for component key: 'temp' (done) | time=60ms
INFO: Load quality profiles
INFO: Load quality profiles (done) | time=140ms
INFO: Load active rules
INFO: Load active rules (done) | time=4021ms
INFO: Load analysis cache
INFO: Load analysis cache (404) | time=43ms
INFO: Load project repositories
INFO: Load project repositories (done) | time=52ms
INFO: Indexing files...
INFO: Project configuration:
INFO: 37 files indexed
INFO: 0 files ignored because of scm ignore settings
INFO: Quality profile for py: Sonar way
INFO: Quality profile for yaml: Sonar way
INFO: ------------- Run sensors on module temp
INFO: Load metrics repository
INFO: Load metrics repository (done) | time=65ms
INFO: Sensor Python Sensor [python]
WARN: Your code is analyzed as compatible with python 2 and 3 by default. This will prevent the detection of issues specific to python 2 or python 3. You can get a more precise analysis by setting a python version in your configuration via the parameter "sonar.python.version"
INFO: Starting global symbols computation
INFO: 6 source files to be analyzed
INFO: 6/6 source files have been analyzed
INFO: Starting rules execution
INFO: 6 source files to be analyzed
INFO: 6/6 source files have been analyzed
INFO: The Python analyzer was able to leverage cached data from previous analyses for 0 out of 6 files. These files were not parsed.
INFO: Sensor Python Sensor [python] (done) | time=1839ms
INFO: Sensor Cobertura Sensor for Python coverage [python]
INFO: Sensor Cobertura Sensor for Python coverage [python] (done) | time=16ms
INFO: Sensor PythonXUnitSensor [python]
INFO: Sensor PythonXUnitSensor [python] (done) | time=4ms
INFO: Sensor JaCoCo XML Report Importer [jacoco]
INFO: 'sonar.coverage.jacoco.xmlReportPaths' is not defined. Using default locations: target/site/jacoco/jacoco.xml,target/site/jacoco-it/jacoco.xml,build/reports/jacoco/test/jacocoTestReport.xml
INFO: No report imported, no coverage information will be imported by JaCoCo XML Report Importer
INFO: Sensor JaCoCo XML Report Importer [jacoco] (done) | time=3ms
INFO: Sensor IaC CloudFormation Sensor [iac]
INFO: 0 source files to be analyzed
INFO: 0/0 source files have been analyzed
INFO: Sensor IaC CloudFormation Sensor [iac] (done) | time=52ms
INFO: Sensor IaC Kubernetes Sensor [iac]
INFO: 19 source files to be analyzed
INFO: 19/19 source files have been analyzed
INFO: Sensor IaC Kubernetes Sensor [iac] (done) | time=189ms
INFO: Sensor JavaScript inside YAML analysis [javascript]
INFO: No input files found for analysis
INFO: Hit the cache for 0 out of 0
INFO: Miss the cache for 0 out of 0
INFO: Sensor JavaScript inside YAML analysis [javascript] (done) | time=31ms
INFO: Sensor CSS Rules [javascript]
INFO: No CSS, PHP, HTML or VueJS files are found in the project. CSS analysis is skipped.
INFO: Sensor CSS Rules [javascript] (done) | time=0ms
INFO: Sensor C# Project Type Information [csharp]
INFO: Sensor C# Project Type Information [csharp] (done) | time=1ms
INFO: Sensor C# Analysis Log [csharp]
INFO: Sensor C# Analysis Log [csharp] (done) | time=23ms
INFO: Sensor C# Properties [csharp]
INFO: Sensor C# Properties [csharp] (done) | time=0ms
INFO: Sensor HTML [web]
INFO: Sensor HTML [web] (done) | time=4ms
INFO: Sensor TextAndSecretsSensor [text]
INFO: 25 source files to be analyzed
INFO: 25/25 source files have been analyzed
INFO: Sensor TextAndSecretsSensor [text] (done) | time=111ms
INFO: Sensor VB.NET Project Type Information [vbnet]
INFO: Sensor VB.NET Project Type Information [vbnet] (done) | time=1ms
INFO: Sensor VB.NET Analysis Log [vbnet]
INFO: Sensor VB.NET Analysis Log [vbnet] (done) | time=20ms
INFO: Sensor VB.NET Properties [vbnet]
INFO: Sensor VB.NET Properties [vbnet] (done) | time=0ms
INFO: Sensor IaC Docker Sensor [iac]
INFO: 1 source file to be analyzed
INFO: 1/1 source file has been analyzed
INFO: Sensor IaC Docker Sensor [iac] (done) | time=161ms
INFO: ------------- Run sensors on project
INFO: Sensor Analysis Warnings import [csharp]
INFO: Sensor Analysis Warnings import [csharp] (done) | time=1ms
INFO: Sensor Zero Coverage Sensor
INFO: Sensor Zero Coverage Sensor (done) | time=15ms
INFO: SCM Publisher SCM provider for this project is: git
INFO: SCM Publisher 26 source files to be analyzed
INFO: SCM Publisher 26/26 source files have been analyzed (done) | time=363ms
INFO: CPD Executor 1 file had no CPD blocks
INFO: CPD Executor Calculating CPD for 5 files
INFO: CPD Executor CPD calculation finished (done) | time=15ms
INFO: Analysis report generated in 124ms, dir size=215.4 kB
INFO: Analysis report compressed in 136ms, zip size=77.2 kB
INFO: Analysis report uploaded in 162ms
INFO: ANALYSIS SUCCESSFUL, you can find the results at: https://u1513319-9000.theiadocker-3-labs-prod-theiak8s-4-tor01.proxy.cognitiveclass.ai/dashboard?id=temp
INFO: Note that you will be able to access the updated dashboard once the server has processed the submitted analysis report
INFO: More about the report processing at https://u1513319-9000.theiadocker-3-labs-prod-theiak8s-4-tor01.proxy.cognitiveclass.ai/api/ce/task?id=AYmr8xG7Ff_9kO6CBJ7g
INFO: Analysis total time: 11.579 s
INFO: ------------------------------------------------------------------------
INFO: EXECUTION SUCCESS
INFO: ------------------------------------------------------------------------
INFO: Total time: 39.300s
INFO: Final Memory: 26M/94M
INFO: ------------------------------------------------------------------------
theia@theiadocker-u1513319:/home/project/wtecc-CICD_PracticeCode$ 
```


# Intepret the result 

> Where is the risk 

service/__init__.py

```js
"""
Service Package
"""
from flask import Flask
app = Flask(__name__)
Make sure disabling CSRF protection is safe here.
# This must be imported after the Flask app is created
from service import routes               # pylint: disable=wrong-import-position,cyclic-import
from service.common import log_handlers  # pylint: disable=wrong-import-position
log_handlers.init_logging(app, "gunicorn.error")
app.logger.info(70 * "*")
app.logger.info("  S E R V I C E   R U N N I N G  ".center(70, "*"))
app.logger.info(70 * "*")
```

> What's the risk 
A cross-site request forgery (CSRF) attack occurs when a trusted user of a web application can be forced, by an attacker, to perform sensitive actions that he didn’t intend, such as updating his profile or sending a message, more generally anything that can change the state of the application.

The attacker can trick the user/victim to click on a link, corresponding to the privileged action, or to visit a malicious web site that embeds a hidden web request and as web browsers automatically include cookies, the actions can be authenticated and sensitive.

> Access the risk 

```js
Ask Yourself Whether
The web application uses cookies to authenticate users.
There exist sensitive operations in the web application that can be performed when the user is authenticated.
The state / resources of the web application can be modified by doing HTTP POST or HTTP DELETE requests for example.
There is a risk if you answered yes to any of those questions.

Sensitive Code Example
For a Django application, the code is sensitive when,

django.middleware.csrf.CsrfViewMiddleware is not used in the Django settings:
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
] # Sensitive: django.middleware.csrf.CsrfViewMiddleware is missing
the CSRF protection is disabled on a view:
@csrf_exempt # Sensitive
def example(request):
    return HttpResponse("default")
For a Flask application, the code is sensitive when,

the WTF_CSRF_ENABLED setting is set to false:
app = Flask(__name__)
app.config['WTF_CSRF_ENABLED'] = False # Sensitive
the application doesn’t use the CSRFProtect module:
app = Flask(__name__) # Sensitive: CSRFProtect is missing

@app.route('/')
def hello_world():
    return 'Hello, World!'
the CSRF protection is disabled on a view:
app = Flask(__name__)
csrf = CSRFProtect()
csrf.init_app(app)

@app.route('/example/', methods=['POST'])
@csrf.exempt # Sensitive
def example():
    return 'example '
the CSRF protection is disabled on a form:
class unprotectedForm(FlaskForm):
    class Meta:
        csrf = False # Sensitive

    name = TextField('name')
    submit = SubmitField('submit')
```

> How can i fix it 

```js
Recommended Secure Coding Practices
Protection against CSRF attacks is strongly recommended:
to be activated by default for all unsafe HTTP methods.
implemented, for example, with an unguessable CSRF token
Of course all sensitive operations should not be performed with safe HTTP methods like GET which are designed to be used only for information retrieval.
Compliant Solution
For a Django application,

it is recommended to protect all the views with django.middleware.csrf.CsrfViewMiddleware:
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware', # Compliant
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
and to not disable the CSRF protection on specific views:
def example(request): # Compliant
    return HttpResponse("default")
For a Flask application,

the CSRFProtect module should be used (and not disabled further with WTF_CSRF_ENABLED set to false):
app = Flask(__name__)
csrf = CSRFProtect()
csrf.init_app(app) # Compliant
and it is recommended to not disable the CSRF protection on specific views or forms:
@app.route('/example/', methods=['POST']) # Compliant
def example():
    return 'example '

class unprotectedForm(FlaskForm):
    class Meta:
        csrf = True # Compliant

    name = TextField('name')
    submit = SubmitField('submit')
See
OWASP Top 10 2021 Category A1 - Broken Access Control
MITRE, CWE-352 - Cross-Site Request Forgery (CSRF)
OWASP Top 10 2017 Category A6 - Security Misconfiguration
OWASP: Cross-Site Request Forgery
SANS Top 25 - Insecure Interaction Between Components

```
