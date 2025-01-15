# What the script does
The script allows to build the entire system on your local machine, while selecting only mentioned services to be built  
from your local version, and the rest being the latest from the git rep.  
If needed, in order to setup the system manually see the end of this file.

# How to use
## Requirments
- python interpeter / virtual env manager (conda, ...)
- the yaml package for python. if not installed then run: ` pip install pyyaml `

## Running
- run the script with: ```python local_build_script.py```
- for each *local* service, enter it's name like it is specified in the `compose.yaml` file, and then the path to it's Dockerfile in your local folder, as specified by the scripts prompt
- after entering all the local services, write `None` to the script prompt and the script should run.

# What the script does
The script create a copy of the `compose.yaml` file called `docker-compose.override.yaml` and for each service entered,  
it replaces the `image` attribute with a `build` attribute and adds to it a sub-attribute called `context` which holds the path to the service's Dockerfile, allowing it to build your local version of the service.

# What next?
After running the script once, your new compose file remains in your system, and you can still use it (only if you don't want to change what is being built locally).
To run the new compose file directly, use the following commands:
```bash
docker-compose -f compose.yaml -f docker-compose.override.yaml build  
docker-compose -f compose.yaml -f docker-compose.override.yaml up
```


# Manual setup
In Setup repo, open the compose file.
For local repos, replace the image: `<some url>` tag with:
```
build:
context: <local repo path relative to the compose file>
dockerfile: Dockerfile
```
If trying to use local Tekclinic-WebApp repo, ensure you have valid keys and the relevant environment variables in your .env.local file.  
Some issues may arise with the docker files trying to install git and go modules. In order to fix, change the erroneous Dockerfile parts (no need to change in files that did not raise errors).
For git problems you will probably need to comment these line from the Doctors-Microservice Dockerfile:
```
RUN apk add --no-cache git ,
ARG GITHUB_ACTOR 
ARG GITHUB_TOKEN
RUN git config --global url."https://${GITHUB_ACTOR}:${GITHUB_TOKEN}@github.com/".insteadOf "https://github.com/"
``` 
For go modules you will probably need to change the Doctors-Microservice Dockerfile as follows:
comment: `RUN go mod download`
write instead: `RUN GOPROXY="https://goproxy.io" go mod download` 
