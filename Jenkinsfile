def images = [
  [name: "prowlarr", dir: "docker-files/prowlarr", buildArgs: "--build-arg VERSION=1.25.4.4818 --build-arg BRANCH=master"],
  [name: "radarr", dir: "docker-files/radarr", buildArgs: "--build-arg VERSION=5.14.0.9383 --build-arg BRANCH=master"],
  [name: "sonarr", dir: "docker-files/sonarr", buildArgs: "--build-arg VERSION=4.0.10.2544 --build-arg BRANCH=main"],
  [name: "jellyseerr", dir: "docker-files/jellyseerr", buildArgs: ""],
  [name: "jellyfin", dir: "docker-files/jellyfin", buildArgs: "--build-arg JELLY_VERSION=10.10.1 --build-arg JFFMPEG_VERSION=7.0.2-6 --build-arg JELLY_BRANCH=stable"]
]
def builtImages = [: ]

pipeline {
  agent any
  environment {
    registry = "kvait"
    registryCredential = "dockerhub"
    GITHUB_TOKEN = credentials("github-jenkins-access-token")
    GITHUB_REPO = "DarkMurky/arr-media-server"
  }

  stages {
    stage("Checkout Code") {
      steps {
        script {
          env.GITHUB_COMMIT = env.GIT_COMMIT
          echo "Triggered branch: ${GIT_BRANCH.split("/")[1]}"

          updateGitHubStatus('pending', 'Job is building...')
        }
        git branch: "${GIT_BRANCH.split("/")[1]}",
          url: "https://github.com/${GITHUB_REPO}.git"
      }
    }

    stage("Build Images") {
      steps {
        script {
          for (image in images) {
            def name = image.name
            def dirPath = image.dir
            def buildArgs = image.buildArgs

            dir(dirPath) {
              builtImages[name] = docker.build("${registry}/${name}:${params.environment}", "${buildArgs} .")
            }
          }
        }
      }
    }

    stage("Deploy Images") {
      steps {
        script {
          docker.withRegistry("", registryCredential) {
            for (image in images) {
              def name = image.name
              builtImages[name].push("${params.environment}")
            }
          }
        }
      }
    }

    stage("Remove Unused Docker Images") {
      steps {
        script {
          for (image in images) {
            def name = image.name
            sh "docker rmi ${registry}/${name}:${params.environment}"
          }
        }
      }
    }
  }

  post {
    success {
      script {
        updateGitHubStatus("success", "Build succeeded")
      }
    }
    failure {
      script {
        updateGitHubStatus("failure", "Build failed")
      }
    }
  }
}

def updateGitHubStatus(String state, String description) {
  def url = "https://api.github.com/repos/${GITHUB_REPO}/statuses/${GITHUB_COMMIT}"
  def json = """
  {
    "state": "${state}",
    "description": "${description}",
    "context": "continuous-integration/jenkins"
  }
  """

  httpRequest(
    url: url,
    httpMode: "POST",
    contentType: "APPLICATION_JSON",
    requestBody: json,
    customHeaders: [
      [name: "Authorization", value: "token ${GITHUB_TOKEN}"]
    ]
  )
}
