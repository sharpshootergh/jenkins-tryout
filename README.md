# jenkins-tryout
**Pipeline Overview**

This Jenkins pipeline is designed for a Maven project using GitLab as the repository management tool. It includes various stages such as checkout, versioning, building, testing, packaging, E2E testing, and publishing. The pipeline also handles different branches including main, feature/*, and release/*.

**Pipeline Configuration**

Agent
The pipeline runs on any available agent.

**Options**

timestamps(): Adds timestamps to the console output.
timeout(time:15, unit:'MINUTES'): Sets a timeout of 15 minutes for the pipeline.
gitLabConnection('gitlab'): Uses a GitLab connection named 'gitlab'.
**Triggers**
The pipeline is triggered by GitLab events:

triggerOnPush: Triggered on push events.
triggerOnMergeRequest: Triggered on merge requests.
branchFilterType: 'All': Applies to all branches.
**Tools**
Maven version 3.6.2
JDK version 8
Pipeline Stages
1. Checkout
Purpose: Checks out the source code from SCM.
Steps: checkout scm
2. Version
Condition: Runs only on release/* branches.
Purpose: Sets a new version number.
Steps:
Retrieves the major and minor version from the branch name.
Gets the last version tag.
Calculates the next patch version.
Updates the project version using Maven.
3. Build Test
Condition: Runs only on the main branch.
Purpose: Runs verification tests.
Steps: Uses Maven to verify the project.
4. Package
Condition: Runs on feature/* and release/* branches.
Purpose: Packages the project.
Steps: Uses Maven to package the project.
5. E2E (End-to-End Testing)
Condition: Runs on main and release/* branches.
Purpose: Performs end-to-end tests.
Steps:
Downloads necessary JAR files from Artifactory.
Sets up and runs the simulation using Java.
6. Publish
Condition: Runs on release/* and main branches.
Purpose: Deploys the project.
**Steps:**

If on main branch, deploys the project with Maven.
If on release/* branch, updates the version, tags the release, and pushes the tags to the repository.
**Post Actions**

Always:
Cleans the workspace.
Sends an email notification with the build status.
Success: Updates the GitLab commit status to 'success'.
Failure: Updates the GitLab commit status to 'failed'.
**Helper Functions**
formatTag(_3_number_version)
Formats a version tag.

getNext3NumberVersion(tag)
Calculates the next patch version based on the given tag.

getMajorMinorVersion(release_branch)
Extracts the major and minor version from the release branch name.

getLastVersionTag(major_minor_version)
Retrieves the last version tag for the given major and minor version.

**Example Usage**

To use this pipeline, simply define it in your Jenkinsfile and ensure your GitLab and Maven configurations match the specified settings.

