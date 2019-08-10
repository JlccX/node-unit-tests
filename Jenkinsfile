pipeline {
    
    agent none;
    
    stages {
        
        stage("Build") {
            steps {
                script {
                    node {
                        println "Build stage"
                    }
                }
            }
        }
        
         stage("Unit tests") {
            steps {
                script {
                    node {
                        println "unit tests stage"
                    }
                }
            }
        }
        
         stage("sonar analysis") {
            steps {
                script {
                    node {
                        println "analisis de codigo con Sonarqube"
                    }
                }
            }
        }
        
        
    }
    
    
}
