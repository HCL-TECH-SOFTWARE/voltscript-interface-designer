# Voltscript Interface Designer

VoltScript Interface Designer, the application formerly known as the LSX Toolkit. This repo will hold everything needed to use the application and documentation.

## Issues

Issues should be created using GitHub issues on HCL Software GitHub.

## Documentation

Documentation uses [Material for MKDocs](https://squidfunk.github.io/mkdocs-material/getting-started/#installation), which uses Markdown files in the docs directory.

This can be previewed using a Docker container or locally. To run locally, follow the [Material for MKDocs documentation](https://squidfunk.github.io/mkdocs-material/getting-started/#with-pip) and the steps to install the plugins used referenced in mkdocs.yml.

To use with Docker, a Dockerfile has been included. Open a terminal in this directory. Run the relevant "makedocker" file for your operating system. This creates a Docker image based on MKDocs Material image, adding the additional plugin.

Once the Docker container is created, it can be started as a temporary container by:

1. Open a terminal and navigate to this directory.
2. Issue the command `docker run --rm -it -p 8000:8000 -v ${PWD}:/docs voltscript-docs`. On Windows, `${PWD}` maps to the current directory for PowerShell, `%cd%` for a normal Windows cmd prompt. Other MKDocs commands can be appended, as required - see MKDocs documentation for more details.
