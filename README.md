# Conan dependency building
Contains all configuration files for building IncludeOS dependencies.
The build jobs in this repository are for 3rd party external dependencies in use by IncludeOS.


## Creating/adding new build jobs
To create a new build job:
Place a `Jenkinsfile` and a `conanfile.py` in the correct directory structure.
The Jenkinsfile should contain:
- Parameters that the dependency can be built with.
	- **IMPORTANT:** Make the choice parameter always have an empty item as the first choice. This is needed when initializing the jobs.
- A stage that builds and calls the external
For reference have a look at one of our structures.

### Job build Workflow
1. Root Jenkinsfile: JobDSL to create 1 **Unique** build job pr dependency.
2. Each dependency needs to have a Jenkinsfile and a conanfile.py
3. Root Jenkinsfile: Scans the includeos/conan repo for all Jenkinsfiles excluding itself.

Any changes made to the Jenkinsfile requires the seed-job to be run and the scripts accepted by the admin before the jobs are built.

### Dependency Workflow
The job pipeline for each dependency contains 2 stages:
- Build
- Upload

The jobs require a number of parameters;
- **Version:** _**choose** a version_
- **Upload_channel:** *name a channel you would like to add/create package to*
- **Profiles:** *available profile names are specified*
- **Target_os*:** *currently available for: __Macos__ or __Linux__. __(can be left empty)__*
- **Target_architectures*:** *currently available for: __x86_64__ or __x86__ __(can be left empty)__*
- **Build_types:** *Two available build types specified; __Debug__ and __Release__*

The **Build** step is capable of building a number of packages parallely depending on the instance handling the builds.

The **Upload** step uploads the packages, if the build step for all the packages specified is successful. The packages are uploaded to [includeos bintray](https://bintray.com/includeos)



> **Note:** For parameters marked with a [**\***], these settings are included in the Profiles, entering them here overwrites the profiles default values. Please have a look at the settings in the profile for build compatibility before attempting to change settings.


**Upcoming step:** *To Automate a test build workflow by triggering a dependency build for a PR made to that dependency repo*
