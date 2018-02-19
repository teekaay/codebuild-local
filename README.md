# Codebuild Local

`codebuild-local` is a simple script for running
AWS CodeBuild projects locally by using its `buildspec.yaml` file format.

## How it works

Basically, `codebuild-local` takes a given `buildspec.yaml` 
and converts the definition into a dockerfile. Afterwards,
a docker container is built and the build specification is executed within the container.

## Getting Started

To install the script

    git clone https://github.com/teekaay/codebuild-local.git
    cd codebuild-local
    pip install -e .

To run a code build specificiation (using the demo `codebuild.yaml` from the project)

    codebuild-local --rm

For all available options, run `codebuild-local --help`.

## Usage 

    usage: codebuild-local [-h] [--docker-image DOCKER_IMAGE]
                        [--dockerfile DOCKERFILE] [--buildspec BUILDSPEC]
                        [--context-dir CONTEXT_DIR] [--project PROJECT] [--rm]
                        [--verbose]

    Run buildspec yaml files locally in docker

    optional arguments:
    -h, --help            show this help message and exit
    --docker-image DOCKER_IMAGE
                            Name of the docker image to use. Defaults to debian
    --dockerfile DOCKERFILE
                            Path to the generated Dockerfile. If not specified,
                            the Dockerfile will be printed to stdout
    --buildspec BUILDSPEC
                            Path to buildspec file. Defaults to buildspec.yaml
    --context-dir CONTEXT_DIR
                            Docker context directory. Defaults to current working
                            directory
    --project PROJECT     Name of the project. If not provided a random UUID
                            will be generated
    --rm                  If set, intermediate files will be cleaned up
    --verbose             Enable verbose logging


## Further Notes

### Using AWS SSM

If you want to use AWS for retrieving parameters from the parameter store, you can do this by using the `env` propery

    env:
        variables: {}
        parameter-store: 
            key1: "value1"
            key2: "value2"

Here, the two SSM parameters `value1` and `value2` will be retrieved from SSM and be set as 

    ENV key1=<value-of-ssm-param-value2>
    ENV key2=<value-of-ssm-param-value2>

**Note that you need to have a valid AWS session token.**