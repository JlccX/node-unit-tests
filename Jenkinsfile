pipeline {
    
    agent none;
    
    environment {
        myUsername = "JoseLuis"
    }
    
    stages {
        
        stage("Build") {
            steps {
                script {
                    node {
                        println "Build stage "
                        echo "Nombre de Usuario ${myUsername}"
                    }
                }
            }
        }
        
         stage("Unit tests") {
             environment {
                 myUsername = "JlccX"
             }
            steps {
                script {
                    node {
                        println "unit tests stage"
                        println "Nombre de Usuario $myUsername"
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
        
         stage("deploy") {
            steps {
                script {
                    node {
                        println "deploy xyz 3"
                    }
                }
            }
        }
        
        
    }
    
    
}
