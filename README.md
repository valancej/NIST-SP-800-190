# Anchore and NIST SP 800-190

## Introduction

The National Institute of Standards and Technology Special Publication 800-190 was designed to explain the security concerns associated with container technologies. Along with this, recommendations are made thoughout the publication for addressing the outlined concerns. 

Below details how Anchore can help with execution and mapping of section 4.1 Image Countermeasures.

## 4.1 Image Countermeasures

###### 4.1.1 Image Vulnerabilities

Anchore Engine will scan images and create a manifest of packages. From this manifest, there is the ability to run checks for image vulnerabilites. In addition,the ability to periodically check if new vulnerabilies have been published that directly impact a package contained within a relevant image manifest. 

Anchore has the ability to be integrated with common CI/CD tools (Jenkins), or in an adhoc manner from a command line. From these integrations, policy checks can be enforced to potentially fail builds. 

Anchore can be integrated with an on-premise image registry, public or private Dockerhub, AWS, Azure, Artifactory, etc..

Anchore provides image checks and vulnerability info for the following base layers:

- Alpine
- Ubuntu
- CentOS
- Debian
- Red Hat Enterprise Linux

Anchore provides image checks and vulnerability info for the following third party packages contained within images:

- NodeJS
- Ruby
- Python
- Java


###### 4.1.2 Image Configuration Defects

Policies can be created that enforce Dockerfile and image best practices. As an example, Anchore allows the ability to look for a base image to be in existance via a regex check. These regular expressions can be used to enforce policies specific to image layers, files, etc..

Anchore provides a mechanism for stopping images built from Dockerfiles that have exposed port 22. As outlined in the NIST publication, SSH and other remote administration tools that provide remote access should never be enabled within containers. In this case, an Anchore policy check can be set up to block this image if it exposes port 22.

A common security best practice is reduce attack surface areas, in the case of Docker images, having a list of base layers from minimilistic technologies (Alpine Linux). An Anchore policy can be configured to check for certain base image layers via a FROM statement existing in a Dockerfile. 

Other use cases for Anchore policy checks:

- Dockerfiles must have USER
- Dockerfiles must have HEALTHCHECK

###### 4.1.4 Embedded clear text secrets

Clear text secrets should never be present inside of images. They should be stored outside of the image and provided dynamically at runtime as needed. 

Anchore provides mechanisms to search for secrets that may be contained within an image via a regex check (filename or content). The common ones availble by default are:

- AWS_ACCESS_KEY
- AWS_SECRET_KEY
- PRIV_KEY
- DOCKER_AUTH
- API_KEY

###### 4.1.5 Use of untrusted images

As outlined in the publication, organization should maintain a set of trusted images and registries. By only allowing trusted images to be used, the risk of untrusted, vulnerable images is mitigated. There are several tools that can be used to help with achieving this: 

- Anchore
- Notary
- Jenkins
- Docker Registry

More details around how to setup a secure container based CI/CD Pipeline here: https://anchore.com/blog/container-security-cicd-pipeline-open-source/

###### 4.2.2 Stale images in the registry

There are two ways to mitigate the risk of stale images being used: 

- Organizations can prune registries that contain vulnerable images
- Best practices should be collectively agreed upon to only allow access to images that are fresh. 

Within Anchore, a policy check can be set up to check for a tag of an image that has not been deemed stale. As an example, if a development team builds a my-app:latest image inheriting from my-base-layer:latest within their trusted registry that they have built, an Anchore Dockerfile check can for my-base:latest to be present. Along with the appropriate amount of automation and checks,the risk of using stale and older images is greatly decreased.  

###### 4.4.1 Vulnerabilities within the runtime software

While this can only partly handled by Anchore itself, it is important for organizations to shift security check as far left as possible in the development lifecycle in order to catch Common Vulnerabilities and Exposures (CVEs) prior to a container being deployed. Anchore provides policy gates and checks for 3rd party packages (NPM, GEM, JAVA, PY), in order to provide insight into any potential vulnerabilites. Setting up these checks is a good place to start. 