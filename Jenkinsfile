node {

     // les variables ENV 
    
    //env.STAGE = STAGE 
    //env.TAG = TAG 
    //env.REINSTALL_PROJECT = REINSTALL_PROJECT 
    //env.DELETE_VENDOR = DELETE_VENDOR 
    //env.GENERATE_ASSETS = GENERATE_ASSETS 
    //env.DEPLOY = DEPLOY 

    try { 
        // Mise à jour de déploiement 
        checkout scm 
        stage("Tool Setup") {
            sh "php -v" 
            // Compositeur deps comme deployer 
            sh "pwd"
            sh "php composer.phar install" 
            // Phing 
            if (! fileExists ('phing-latest.phar')) { 
                sh "curl -sS -O https://www.phing.info/get/phing-latest.phar -o '/opt/phing' " 
            } 
            sh "phing -v"
            sh "phing"
            sh "printenv"
        }
       
    } catch (err) { 
        currentBuild.result = 'FAILURE' 
        throw err 
    } 
}
