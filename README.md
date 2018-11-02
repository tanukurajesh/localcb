# `localcb`

Run AWS CodeBuild pipeline directly on your local machine using Docker containers.

## Installation

```bash
go get -u -v github.com/piotrkubisa/localcb/cmd/localcb/
```

Additional requirements:

* Docker client.
* Add `$GOPATH/bin` to your `$PATH` (to run `localcb` without the need to provide absolute path to the statically linked binary).

## AWS CodeBuild provider

[AWS CodeBuild](https://aws.amazon.com/codebuild/) uses `buildspec.yml` file as the [definition](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html) of the stages (named as phases) and shell commands.
It uses single Docker image during the runtime.
Developers might use official, [AWS CodeBuild curated Docker images](https://github.com/aws/aws-codebuild-docker-images) or create own to meet their demands.

### Prerequisites

In most of cases one of [AWS CodeBuild curated Docker image](https://github.com/aws/aws-codebuild-docker-images) will be used, that's why you probably will need to clone repository and build them on your local machine. I recommend to navigate to the [official repository](https://github.com/aws/aws-codebuild-docker-images) for more information.

```bash
git clone https://github.com/aws/aws-codebuild-docker-images.git
cd aws-codebuild-docker-images
cd ubuntu/docker/17.09.0
docker build -t aws/codebuild/docker:17.09.0 .
```

### Usage

The simplest form of `localcb` command with `AWS CodeBuild` requires providing `--image` flag with a name of the Docker image, which are going to be used to run all shell commands inside.

```bash
localcb run --image aws/codebuild/docker:17.09.0
```

`localcb` will load `buildspec.yml` file, parse all defined phases and shell commands, create `localcb.sh` file and then (unless `--dry-run` flag was provided) it will start docker container (with mounted volume and with bind env-variables) and execute aforementioned `localcb.sh` file.

For more information it is recommended to inspect `localcb codebuild --help`.

## Credits

During creating initial version of `localcb` I has been inspired by the well-known [awslabs/aws-sam-local](https://github.com/awslabs/aws-sam-local) to resemble its logic and create a sample application which parses a AWS CodeBuild's definition and run it on my local machine.
