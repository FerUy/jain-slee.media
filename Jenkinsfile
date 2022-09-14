pipeline {
    agent any

	tools {
	    jdk 'JDK 11'
		maven 'Maven_3.8.5'
	}
	parameters { string(name: 'JAIN_SLEE_MEDIA_MAJOR_VERSION', defaultValue: '7.2.0', description: 'The major version for JAIN SLEE MEDIA version') }

    environment{
        MEDIA_BUILD_VERSION = "${params.JAIN_SLEE_MEDIA_MAJOR_VERSION}-${BUILD_NUMBER}"
    }

	stages{
		stage("Build ") {
			steps {
			    sh 'java -version'
				script {
				    MEDIA_BUILD_VERSION = "${params.JAIN_SLEE_MEDIA_MAJOR_VERSION}-${BUILD_NUMBER}"
                    currentBuild.displayName = "#${MEDIA_BUILD_VERSION}"
                    currentBuild.description = "JAIN SLEE MEDIA (${env.BRANCH_NAME})"
                }
				echo "Building JAIN SLEE MEDIA version (#${MEDIA_BUILD_VERSION})"
                sh "mvn versions:set -DgenerateBackupPoms=false -DnewVersion=${MEDIA_BUILD_VERSION} clean install -DskipTests"
				sh 'mvn clean install -DskipTests'
                echo "Maven build completed."
            }
        }

		stage("Release") {
			steps {
				echo "Building a wildfly release version of #${MEDIA_BUILD_VERSION}"
                sh 'find . -type d -name \\"target\\" -exec r -r {} +'
                sh "mvn versions:set -DnewVersion=${MEDIA_BUILD_VERSION} clean install -Prelease -Drelease.dir=../../../${MEDIA_BUILD_VERSION} -Dmaven.test.skip=true"
                echo "Building wildfly version completed."
            }
        }
	}
}