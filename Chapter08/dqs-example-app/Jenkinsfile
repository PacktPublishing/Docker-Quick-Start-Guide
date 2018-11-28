node {
   def app
   stage('Clone repository') {
      /* Clone the repository to our workspace */
      checkout scm
   }
   stage('Build image') {
      /* Builds the image; synonymous to docker image build on the command line */
      /* Use a registry name if pushing into docker hub or your company registry, like this */
      /* app = docker.build("earlwaud/dqs-example-app") */
      app = docker.build("dqs-example-app")
   }
   stage('Test image') {
      /* Execute the defined tests */
      app.inside {
         sh 'npm test'
      }
   }
   stage('Push image') {
      /* Now, push the image into the registry */
      /* This would probably be docker hub or your company registry, like this */
      /* docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') */

      /* For this example, We are using our jenkins-stack service registry */
      docker.withRegistry('https://ubuntu-node00:5000') {
         app.push("latest")
      }
   }
}
