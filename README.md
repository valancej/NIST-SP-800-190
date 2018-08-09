# NIST-SP-800-190

## 4.1 Image Countermeasures

###### 4.1.1 Image Vulnerabilities

Anchore Engine will scan images and create a manifest of packages. From this manifest, there is the ability to run checks for image vulnerabilites. Along with this, the ability to periodically check if new vulnerabilies have been published that directly impact a package contained within a relevant image manifest. 

Anchore has the ability to be integrated with common CI/CD tools (Jenkins, CircleCI, etc.), or in an adhoc manner from a command line. From these integrations, policy checks can be enforced to potentially fail builds. 

Depending on overall container footprint, Anchore checks will provide the most value through a proper CI/CD model/pipeline. Having the ability to split up acceptable base images and application layers is critical for appropriate security abstraction. Multiple Anchore gates specific to each of these image layers is fundamental. As an example, prior to trusted base image promotion and push into a binary repo, it will need to pass Anchore checks for Dockerfile best practices (USER, non ssh open), and operating system package vulnerability checks. 

Secondary to the above, once a set of base images have been signed (Notary) and push into a trusted repo, it is now a requirement for all 'application specific' images to be created. It is the responsibility of whoever is building these images to make sure the appropriate base images are being used. Inheritance will apply here, and only signed images from the trusted repo will be able to pass the next set of Anchore policy checks. These checks will not only focus on the signed and approved base layer images, but depending on the application layer dependencies, will check for any npm or python packages that contain published vulns. 

###### 4.1.2 Image Configuration Defects

Policies can be created that enforce Dockerfile and image best practices. As an example, Anchore allows the ability to look for a base image to be in existance via a regex check. These regular expressions can be used to enforce policies specific to image layers, files, etc..