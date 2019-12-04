# Docker image with Terraform/Terragrunt and all needed components to easily manage AWS infrastructure.

[![GitHub krzysztof-szyper-epam/docker-terragrunt](https://img.shields.io/badge/github-krzysztof--szyper--epam%2Fdocker--terragrunt-blue.svg)](https://github.com/krzysztof-szyper-epam/docker-terragrunt "shields.io")
[![Actions Status](https://github.com/Krzysztof-Szyper-Epam/docker-terragrunt/workflows/On%20commit%20push/badge.svg)](https://github.com/Krzysztof-Szyper-Epam/docker-terragrunt/actions?query=workflow%3A%22On+commit+push%22 "github.com")
[![Actions Status](https://github.com/Krzysztof-Szyper-Epam/docker-terragrunt/workflows/On%20pull%20request/badge.svg)](https://github.com/Krzysztof-Szyper-Epam/docker-terragrunt/actions?query=workflow%3A%22On+pull+request%22 "github.com")
![GitHub](https://img.shields.io/github/license/krzysztof-szyper-epam/docker-terragrunt "shields.io")
<br>
![GitHub last commit](https://img.shields.io/github/last-commit/krzysztof-szyper-epam/docker-terragrunt "shields.io")
[![GitHub file size in bytes](https://img.shields.io/github/size/krzysztof-szyper-epam/docker-terragrunt/Dockerfile?label=Dockerfile)](https://hub.docker.com/r/christophshyper/docker-terragrunt "shields.io")
![GitHub code size in bytes](https://img.shields.io/github/languages/code-size/krzysztof-szyper-epam/docker-terragrunt "shields.io")
![GitHub repo size](https://img.shields.io/github/repo-size/krzysztof-szyper-epam/docker-terragrunt "shields.io")
![GitHub language count](https://img.shields.io/github/languages/count/krzysztof-szyper-epam/docker-terragrunt "shields.io")
![GitHub top language](https://img.shields.io/github/languages/top/krzysztof-szyper-epam/docker-terragrunt "shields.io")
<br>
[![Docker Pulls](https://img.shields.io/docker/pulls/christophshyper/docker-terragrunt)](https://hub.docker.com/r/christophshyper/docker-terragrunt "shields.io")
[![Docker Stars](https://img.shields.io/docker/stars/christophshyper/docker-terragrunt)](https://hub.docker.com/r/christophshyper/docker-terragrunt "shields.io")
[![MicroBadger Image](https://images.microbadger.com/badges/image/christophshyper/docker-terragrunt.svg)](https://microbadger.com/images/christophshyper/docker-terragrunt "Get your own image badge on microbadger.com")
[![MicroBadger Version](https://images.microbadger.com/badges/version/christophshyper/docker-terragrunt.svg)](https://microbadger.com/images/christophshyper/docker-terragrunt "Get your own version badge on microbadger.com")
[![MicroBadger Commit](https://images.microbadger.com/badges/commit/christophshyper/docker-terragrunt.svg)](https://microbadger.com/images/christophshyper/docker-terragrunt "Get your own commit badge on microbadger.com")
<br>
[![dockeri.co](https://dockeri.co/image/christophshyper/docker-terragrunt)](https://hub.docker.com/r/christophshyper/docker-terragrunt "dockeri.co")


**Docker image is available at [DockerHub](https://hub.docker.com/) under [christophshyper/docker-terragrunt](https://hub.docker.com/repository/docker/christophshyper/docker-terragrunt).**
<br>
Tag of Docker image tells which version of Terraform and Terragrunt it's bundled with.
<br>
For example `christophshyper/docker-terragrunt:0.12.17-0.21.7` means it's Terraform v0.12.17 and Terragrunt v0.21.7.

**Source code is available at [GitHub](https://github.com/) under [Krzysztof-Szyper-Epam/docker-terragrunt](https://github.com/Krzysztof-Szyper-Epam/docker-terragrunt).**

Dockerfile is based on two images made by [cytopia](https://github.com/cytopia): [docker-terragrunt](https://github.com/cytopia/docker-terragrunt/tree/1bc1a2c6de42c6d19f7e91f64f30256c24fd386f) and [docker-terragrunt-fmt](https://github.com/cytopia/docker-terragrunt-fmt/tree/3f8964bea0db043a05d4a8d622f94a07f109b5a7). 
<br>
Their original README files are included in this repository: [docker-terragrunt](https://github.com/Krzysztof-Szyper-Epam/docker-terragrunt/blob/master/README.docker-terragrunt.md) and [docker-terragrunt-fmt](https://github.com/Krzysztof-Szyper-Epam/docker-terragrunt/blob/master/README.docker-terragrunt-fmt.md).
<br>
Some changes have been applied to add more software to the image - list below.

# Usage
Mount working directory under `/data`.

For example:
```bash
# Format all HCL files in current directory.
docker run --rm \ 
    -u $(id -u):$(id -g) \
    -v $(pwd):/data \
    -w /data \
    christophshyper/docker-terragrunt format.hcl

# Plan terraform deployment
docker run --rm \
    -ti \
    -e AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} \
    -e AWS_SECRET_ACCESS_KEY={$AWS_SECRET_ACCESS_KEY} \
    -e AWS_SESSION_TOKEN={$AWS_SESSION_TOKEN} \
    -u $(id -u):$(id -g) \
    -v $(pwd):/data \
    -w /data/infra \
    christophshyper/docker-terragrunt terraform plan

# Apply terragrunt deployment
docker run --rm \
    -e AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} \
    -e AWS_SECRET_ACCESS_KEY={$AWS_SECRET_ACCESS_KEY} \
    -e AWS_SESSION_TOKEN={$AWS_SESSION_TOKEN} \
    -u $(id -u):$(id -g) \
    -v $(pwd):/data \
    -w /data/infra \
    christophshyper/docker-terragrunt terragrunt apply
```


# Available scripts
* format-hcl.sh
    * For formatting HCL files into format suggested by [Hashicorp](https://github.com/hashicorp/hcl).
    * Using [cytopia's](https://github.com/cytopia) [terragrunt-fmt.sh](https://github.com/cytopia/docker-terragrunt-fmt) (it can also only output list of files or needed diffs) plus additionally calling `terraform fmt`.
    * Will search for fall HCL files (`.hcl`, `.tf` and `.tfvars`) recursively in work directory and modify them automatically by default.


# Available binaries
* [awscli](https://github.com/aws/aws-cli) - For interacting with AWS infrastructure, e.g. for publishing Lambda packages to S3.
* [bash](https://www.gnu.org/software/bash/) - For color output from `terraform` and`terragrunt`. Assures also access to some builtins.
* [curl](https://curl.haxx.se/) - For interacting with [ElasticSearch](https://github.com/elastic/elasticsearch) and [Kibana](https://github.com/elastic/kibana).
* [docker](https://github.com/docker/docker-ce) - For running another container, e.g. for deploying Lambdas with [LambCI's](https://github.com/lambci) [docker-lambda](https://github.com/lambci/docker-lambda).
* [git](https://git-scm.com/) - For interacting with [Github](https://github.com) repositories.
* [jq](https://stedolan.github.io/jq/) - For parsing JSON outputs of [awscli](https://github.com/aws/aws-cli).
* [make](https://www.gnu.org/software/make/) - For using `Makefile` instead of scripts in deployment process.
* [openssl](https://github.com/openssl/openssl) - For calculating BASE64SHA256 hash of Lambda packages. Assures updating Lambdas only when package hash changed.
* [python3](https://www.python.org/) - For running more complex scripts during deployment process.
* [scenery](https://github.com/dmlittle/scenery) - For better coloring and visualization of `terraform plan` outputs.
* [terraform](https://github.com/hashicorp/terraform) - For managing IaC. Dependency for [Terragrunt](https://github.com/gruntwork-io/terragrunt). 
* [terragrunt](https://github.com/gruntwork-io/terragrunt) - For managing IaC. Wrapper over [Terraform](https://github.com/hashicorp/terraform).
* [zip](http://infozip.sourceforge.net/) - For creating packages for Lambdas.


# Available Python libraries
* [ply](https://github.com/dabeaz/ply) - Dependency for [pyhcl](https://github.com/virtuald/pyhcl)
* [pyhcl](https://github.com/virtuald/pyhcl) - For easily parsing of any file in HCL format, whether it's `.hcl`, `.tfvars` or `.tf`.
