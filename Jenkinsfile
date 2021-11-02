pipeline {
  agent any
 parameters {
        string(name: 'name_container', defaultValue: 'web-server', description: 'nombre del docker')
        string(name: 'name_imagen', defaultValue: 'web-server', description: 'nombre de la imagen')
        string(name: 'tag_imagen', defaultValue: 'latest', description: 'etiqueta de la imagen')
#        string(name: 'puerto_imagen', defaultValue: '8081', description: 'puerto a publicar')
	choice choices: ['8081','8082','8083'], description: '', name: 'PUERTO'
    }
    environment {
        name_final = "${name_container}${tag_imagen}${PUERTO}"        
    }
    stages {
          stage('stop/rm') {

            when {
                expression { 
                    DOCKER_EXIST = sh(returnStdout: true, script: 'echo "$(docker ps -q --filter name=${name_final})"').trim()
                    return  DOCKER_EXIST != '' 
                }
            }
            steps {
                script{
                    sh ''' 
                         docker stop ${name_final}
			 docker rm ${name_final}
                    '''
                    }
                    
                }                    
                                  
            }
           
        stage('build') {
            steps {
                script{
                    sh ''' 
                    docker build ./docker/ -t ${name_imagen}:${tag_imagen}
                    '''
                    }
                    
                }                    
                                  
            }
            stage('run') {
            steps {
                script{
                    sh ''' 
                        docker run -dp ${PUERTO}:80 --name ${name_final} ${name_imagen}:${tag_imagen}
 
                    '''
                    }
                    
                }                    
                                  
            }
            
          
        }   
    }
