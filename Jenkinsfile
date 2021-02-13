pipeline {
    agent any

    stages {
        stage ('Clone') {
            steps {
                git branch: 'master', url: "https://github.com/Ankitp110/GradleProject-TETRANOODLE.git"
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "ARTIFACTORY_SERVER",
                    url: "http://159.65.145:8081/artifactory",
                    credentialsId: 'admin.jfrog'
                )

                rtGradleDeployer (
                    id: "GRADLE_DEPLOYER",
                    serverId: "ARTIFACTORY_SERVER",
                    repo: "libs-release-local",
                )

                rtGradleResolver (
                    id: "GRADLE_RESOLVER",
                    serverId: "ARTIFACTORY_SERVER",
                    repo: "jcenter"
                )
            }
        }

        stage ('Exec Gradle') {
            steps {
                rtGradleRun (
                    tool: Gradle 6.8, // Tool name from Jenkins configuration
                    rootDir: "https://github.com/Ankitp110/GradleProject-TETRANOODLE.git",
                    buildFile: 'build.gradle',
                    tasks: 'clean artifactoryPublish',
                    deployerId: "GRADLE_DEPLOYER",
                    resolverId: "GRADLE_RESOLVER"
                )
            }
        }
        
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "ARTIFACTORY_SERVER"
                )
            }
        }
    }
}
