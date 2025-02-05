**Chapter 6: Docker Continous Integration (CI)**
-
**1. CI Using Docker**
Continuous Integration (CI) is a practice where developers routinely combine their code into one place, and then the code is automatically checked to detect problems. CI helps in finding bugs faster and makes it easier to fix them.

CI/CD combines the development process with testing, allowing developers to work together to build and test their code. Docker, which can integrate with tools such as Jenkins and GitHub, allows developers to ship and test their code automatically. This helps simplify the development process and save time, while allowing developers to remain productive on other projects.
![rk_FNBJAp](https://hackmd.io/_uploads/H1_uAd1JA.png)

- The developer submits a commit to GitHub.
- GitHub uses a webhook to notify Jenkins about the update.
- Jenkins pulls in a GitHub repository, including a Dockerfile describing the image, as well as application and test code.
- Jenkins builds a Docker image on the Jenkins slave node.
- Jenkins instantiates the Docker container on the slave node, and runs the appropriate tests.

**2. Docker Hub Automated Build**
Docker Hub can automatically build images from source code in an external repository and automatically push the built images to the Docker repository.

When you set up automatic builds (autobuilds), you create a list of branches and tags that you want to build into a Docker image. Whenever you submit code to one of these branches in a source repository (such as on GitHub) that matches one of the listed image tags, a new build will be triggered using a webhook. This new build will produce a Docker image that will then be pushed to the Docker Hub registry.

This autobuild allows you to automatically build images from build context stored in the repository. This build context usually consists of a Dockerfile and other necessary files. This has advantages, including:
- The image is built according to the specifications you have specified.
- Dockerfiles are available to anyone who has access to your Docker Hub repository.
- Your repository will always be up to date with automatic code changes.

This automated build supports both public and private repositories on GitHub and Bitbucket.

Build Statuses
**Queued**: Waiting for the image to be built.
**Building**: The image building process is in progress.
**Success**: The image has been successfully built without any problems.
**Error**: There was a problem with your image.
