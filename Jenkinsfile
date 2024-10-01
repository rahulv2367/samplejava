node {
        // Define the image name and variables
	def dockerImage = 'javatechie/devops-integration:latest'
        def trivyReportJson = 'trivy-report.json'
        def trivyReportHtml = 'trivy-report.html' 
    stage('Checkout') {
        // Define the repository URL and branch
        def repoUrl = 'https://github.com/rahulv2367/samplejava.git' // Replace with your GitHub repository URL
        def branch = 'feature' // Change to the branch you want to check out 
        // Checkout the code from the specified GitHub repository
        checkout([$class: 'GitSCM',
            branches: [[name: "*/${branch}"]],
            userRemoteConfigs: [[url: repoUrl]]
        ])
        
        // Optional: Print the current directory and list the files
        sh 'pwd'
        sh 'ls -la'
    }
    
        stage('Build') 
        // Perform Maven build
        // Assuming you have a pom.xml file in the root of the repository
        {
        // Set JAVA_HOME
        env.JAVA_HOME = tool(name: 'jdk17', type: 'jdk') // Replace 'jdk11' with your JDK name
        
        // Add Maven to PATH
        def mvnHome = tool(name: 'Maven', type: 'maven') // Replace 'Maven' with your Maven name
        env.PATH = "${mvnHome}/bin:${env.PATH}"

        // Debug: Print the PATH
        echo "PATH: ${env.PATH}"

        // Perform Maven build
        sh 'mvn clean install'

        }
        
        stage('Check Trivy Version') {
        sh "trivy --version"
                
        }
	stage('Build docker image'){
        sh 'docker build -t javatechie/devops-integration .'
        }
        stage('Scan Docker Image with Trivy') {
        // Scan the Docker image using Trivy and generate a JSON report
        sh "trivy image --format json --output ${trivyReportJson} ${dockerImage}"
        
        // Convert JSON report to HTML format
        // sh "trivy image --format template --template '@/path/to/template.tpl' --output ${trivyReportHtml} ${dockerImage}"
        }
	
    // Further stages can go here
}
