# Bitbucket Server Terraform Provider

This repository is a fork of [liamniou/terraform-provider-bitbucketserver](https://github.com/liamniou/terraform-provider-bitbucketserver) so that Klarna would be able to independently contribute to this provider and publish the provider separately without conflict with the upstream.

[![user guide](https://img.shields.io/badge/-user%20guide-blue)](https://registry.terraform.io/providers/klarna-incubator/bitbucketserver/latest)

This terraform provider allows management of **Bitbucket Server** resources. The bundled terraform bitbucket provider works only for Bitbucket Cloud.

## Using the provider

See [User Guide](https://registry.terraform.io/providers/klarna-incubator/bitbucketserver/latest) for details on all the provided data and resource types.

### Example

```hcl
provider "bitbucketserver" {
  server   = "https://mybitbucket.example.com"
  username = "admin"
  password = "password"
}

resource "bitbucketserver_project" "test" {
  key         = "TEST"
  name        = "test-01"
  description = "Test project"
}

resource "bitbucketserver_repository" "test" {
  project     = bitbucketserver_project.test.key
  name        = "test-01"
  description = "Test repository"
}
```

## Development Guide

### Requirements

- [Terraform](https://www.terraform.io/downloads.html) 0.12.x
- [Go](https://golang.org/doc/install) 1.11+
  - correctly setup [GOPATH](http://golang.org/doc/code.html#GOPATH
  - add `$GOPATH/bin` to your `$PATH`
- clone this repository to `$GOPATH/src/github.com/gavinbunney/terraform-provider-bitbucketserver`

### Building the provider

To build the provider, run `make build`. This will also put the provider binary in the `$GOPATH/bin` directory.

```sh
$ make build
```

### Testing

In order to test the provider, you can simply run `make test`.

```sh
$ make test
```

In order to run the full suite of acceptance tests, run `make testacc-bitbucket`.

```sh
$ make testacc-bitbucket
```

Alternatively, you can manually start Bitbucket Server docker container, run the acceptance tests and then shut down the docker.

```sh
$ scripts/start-docker-compose.sh
$ make testacc
$ scripts/stop-docker-compose.sh
```

#### Testing on MacOS

If you try to use `make testacc-bitbucket` locally on MacOS, the command will fail. You can use [act](https://github.com/nektos/act) tool to run acceptance tests via Github actions locally:

```sh
$ act -j testacc -s DOCKERHUB_USERNAME=provide_username -s DOCKERHUB_TOKEN=provide_password
```

### Using the provider locally

```sh
$ make build
$ mkdir -p ~/.terraform.d/plugins/terraform.local/local/bitbucketserver/0.0.99/darwin_amd64/
$ cp ~/go/bin/terraform-provider-bitbucketserver ~/.terraform.d/plugins/terraform.local/local/bitbucketserver/0.0.99/darwin_amd64/
```

Configure terraform to use the provider from local path:

```hcl
terraform {
  required_providers {
    bitbucketserver = {
      source  = "terraform.local/local/bitbucketserver"
      version = "0.0.99"
    }
  }
}
```
