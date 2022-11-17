node {
   def gitcommit
   stage('Verificación SCM') {
     checkout scm
     sh "git rev-parse --short HEAD > .git/commit-id"                        
     gitcommit = readFile('.git/commit-id').trim()
   }
//    stage('test') {
//      nodejs(nodeJSInstallationName: 'nodejs') {
//        sh 'npm install --only=dev'
//        sh 'npm test'
//      }
//    }
   stage('Docker Build & Push') {
     docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
      def nuestraapp = docker.build("verfomed/webserver:${gitcommit}")
      nuestraapp.push()
     }
   }
   stage('run image') {
    docker.image("verfomed/webserver:${gitcommit}").run('-p 8082:80' + ' -d' + ' --name webserver_test')
   }
}