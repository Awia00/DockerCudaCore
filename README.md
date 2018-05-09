# DockerCudaCore
A repository for docker files with CUDA and .NET Core build/run environments

The JenkinsFile are specified to push to a registry and therefore requires that the following environment variables are set: 

| Variable                | Example                 | Description                       |
| ------------------------|:-----------------------:| ---------------------------------:|
| REGISTRY_URL            | http://my.registry.com  | the url to the registry.          |
| REGISTRY_CREDENTIALS    | Awia00                  | the id jenkins credential to use  |


You are welcome to fork and add new dockerfiles or correct the current ones. We will try to accept pull requests quickly.
