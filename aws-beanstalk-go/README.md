# AWS Elastic Beanstalk Go

## Getting Started

### Beanstalk CLI

#### Installation

```
$ brew install aws-elasticbeanstalk
```

#### App Initialization

From your application run:

```
$ eb init -p Docker
```

It will ask you to provide `aws-access-id` and `aws-secret-key` if you have not done so before.

These can be found under `My Account` in the AWS console or under the `IAM` service in `Security Credentials > Access Credentials > Access Keys`

#### Environment Creation

To create an environment use the `create` command

```
$ eb create <environment_name> -p Docker
```

To specify a specific region use the `-r` option. See `$ eb create -h` for all the options.

This will build package your application and build your Dockerfile image. Some points to watch for:

* `EXPOSE` must be set to `8080`

**Go Runtime**

Using `-p Go`, causes the environment to be created with a pre-configured Go runtime. This means that AWS
will ask you to set the following:

* `FROM` directive in your Dockerfile needs to be `golang:1.4.1-onbuild`

#### Configuring

To configure any resource in the instance, create a folder called `.ebextensions`.
This will be the place where all your configuration files will live.
Each configuration file is a yaml parsed file containing configuration for each
resource in the container itself.

## Issues

**When running `eb create` or `eb deploy`**

Error:

> ERROR: Invalid runtime Docker image. Expecting: golang:1.4.1-onbuild, was: google/golang:1.4.1-onbuild. Abort deployment.

Solution:

1. Using Go runtime, we need to set `FROM` directive in the Dockerfile needed to be exactly `golang:1.4.1-onbuild`
1. Setting Docker runtime for the environment allows using any `FROM` value

**Using SSE**

Error:

> When requesting an SSE stream, the response body would not come back right away and when reconnecting the response body would be cached.

Tried:

- Setting `X-Accel-Buffering` Header to `no`

Solution:

> TBD
