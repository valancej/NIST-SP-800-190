# NIST-SP-800-190

## 4.1 Image Countermeasures

###### 4.1.1 Image Vulnerabilities

Anchore Engine will scan images and create a manifest of packages. From this manifest, there is the ability to run checks for image vulnerabilites. Along with this, the ability to periodically check if new vulnerabilies have been published that directly impact a package contained within a relevant image manifest. 

Anchore has the ability to be integrated in to common CI/CD tools (Jenkins, CircleCI, etc.), or in an adhoc manner from a command line. 


###### 4.1.2 Image Configuration Defects

Policies can be created that enforce Dockerfile and image best practices. As an example, Anchore allows the ability to look for a base image to be in existance via a regex check. These regular expressions can be used to enforce policies specific to image layers, files, etc..