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
                    tool: GRADLE_TOOL, // Tool name from Jenkins configuration
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












        pipeline {
    agent any
    # Global tools to be used by the pipeline
    tools { 
	// Global tools to be used by the pipeline
        maven 'maven3.6' 
        jdk 'jdk8' 
    }
    stages {
	stage('Unit Tests') {
	    // Run Unit tests 
            steps{
                sh 'mvn -f maths/pom.xml clean test'
            }
        }	
        stage ('Artifactory configuration') {
            steps {
		// specify Artifactory server
                rtServer (
                    id: "ARTIFACTORY_SERVER",
                    url: "http://artifactory:8081/artifactory",
		    credentialsId: 'admin.jfrog'
                )
		// specify the repositories to be used for deploying the artifacts in the Artifactory
                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: "libs-release-local",
                    snapshotRepo: "libs-snapshot-local"
                )
		// defines the dependencies resolution details
                rtMavenResolver (
                    id: "MAVEN_RESOLVER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: "libs-release",
                    snapshotRepo: "libs-snapshot"
                )
            }
        }
        stage ('Build & Upload Artifact') {
	    // run Maven Build and upload the built artifact to Artifactory
            steps {
                rtMavenRun (
                    tool: "maven3.6", // Tool name from Jenkins configuration
                    pom: 'maths/pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER",
                    resolverId: "MAVEN_RESOLVER"
                )
            }
        }
        stage ('Publish build info') {
	    // Publish the build info in the Artifactory
            steps {
                rtPublishBuildInfo (
                    serverId: "ARTIFACTORY_SERVER"
                )
            }
        }
    }
}