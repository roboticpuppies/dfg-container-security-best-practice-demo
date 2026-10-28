# Container Security Best Practice Workshop

## Introduction
Welcome to this repository. This contains hands-on material for a workshop held by Devops Focus Group Indonesia on 16th March 2025. It demonstrates the use of containers when building and deploying that do not follow security best practices. We also provide the secured versions of them. The build examples live in folders on `main`; the run examples are still separate branches:

| Location         | Purpose                                                             |
| ---------------- | ------------------------------------------------------------------- |
| `insecure/`      | Example of bad version when building a container image.             |
| `secure/`        | Fixed version of building phase.                                    |
| `run/insecure`   | Example of bad version when running or deploying a container image. |
| `run/secure`     | Fixed version of running phase.                                     |

Build with e.g. `docker build -t demo:secure secure/`.

> [!NOTE]
> Due to time constraints, this workshop is limited to building and running containers using Docker, and doesn't cover Kubernetes. We plan to discuss this in another workshop session.

## Final image size comparison

For those who curious, here is the final image sizes between base image flavors

| Image                        | Size   |
| ---------------------------- | ------ |
| Insecure                     | 292MB  |
| Secure + Distroless Non-root | 27MB   |
| Secure + Alpine              | 14.6MB |
| Secure + Scratch             | 6.72MB |