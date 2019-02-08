node{
  stage ('Build') {
    def PWD = pwd();
    git url: 'https://github.com/ratanbekal/spring-petclinic'
 
    withMaven(
        // Maven installation declared in the Jenkins "Global Tool Configuration"
        maven: 'M3',
        // Maven settings.xml file defined with the Jenkins Config File Provider Plugin
        // Maven settings and global settings can also be defined in Jenkins Global Tools Configuration
        mavenSettingsConfig: '1a281eb1-b847-47cf-89ba-8da7b58b776b',
        mavenLocalRepo: '.repository') {
 
      // Run the maven build
      sh "mvn clean install"
 
    } // withMaven will discover the generated Maven artifacts, JUnit Surefire & FailSafe & FindBugs reports...
  }
  stage('S3 Upload') {
            pwd(); //Log current directory
            withAWS(region:'us-east-2',credentials:'d41ae28d-934b-4ec8-b038-8c9529880851') {

                 def identity=awsIdentity();//Log AWS credentials

                // Upload files from working directory 'dist' in your project workspace
                s3Upload(bucket:"elasticbeanstalk-us-east-2-044661814431", workingDir:'target', includePathPattern:'**/*.jar');
            }    
    }
}
