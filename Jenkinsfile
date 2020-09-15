nœud { 
    // les variables ENV 
    env.PWD = PWD () 
    env.STAGE = STAGE 
    env.TAG = TAG 
    env.REINSTALL_PROJECT = REINSTALL_PROJECT 
    env.DELETE_VENDOR = DELETE_VENDOR 
    env.GENERATE_ASSETS = GENERATE_ASSETS 
    env.DEPLOY = DEPLOY 

    try { 
        // Mise à jour de déploiement 
        checkout scm 
        stage 'Tool Setup' 
        sh "$ {phpBin} -v" 
        // Compositeur deps comme deployer 
        sh "composer.phar install" 
        // Phing 
        if (! fileExists ('phing-latest.phar')) { 
            sh "curl -sS -O https://www.phing.info/get/phing-latest.phar -o $ {phingBin} " 
        } 
        sh" $ {phingCall} -v " 
        sh" printenv "

        stage 'Magento Setup' 
        // Setup and update Magento 
        stage 'Magento Setup' 
        if (! fileExists ('shop')) { 
            sh "git clone $ {magentoGitUrl} shop" 
        } else { 
            dir ('shop') { 
                sh "git fetch origin" 
                sh "git checkout -f $ { TAG} " 
                sh" git reset --hard origin / $ {TAG} " 
            } 
        } 
        dir ('shop') { 
            sh" $ {phingCall} jenkins: flush-all " 
            sh" $ {phingCall} jenkins: setup-project " 
            sh "$ {phingCall} jenkins: flush-all" 
        }
        
        stage 'Asset Generation' 
        dir ('shop') { 
            if (GENERATE_ASSETS == 'true') { 
                sh "$ {phingCall} deploy: passage en mode production" 
                sh "$ {phingCall} deploy: compile" 
                sh " $ {phingCall} deploy: static-content " 
                sh" bash bin / build_artifacts_compress.sh " 
        
                archiveArtifacts 'config.tar.gz' 
                archiveArtifacts 'var_di.tar.gz' 
                archiveArtifacts 'var_generation.tar.gz' 
                archiveArtifacts 'pub_static.tar.gz ' 
                archiveArtifacts' shop.tar.gz ' 
            } 
        }

        stage 'Deployment' 
        if (DEPLOY == 'true') { 
            sshagent (credentials: [jenkinsSshCredentialId]) { 
                sh "./dep deploy --tag = $ {TAG} $ {STAGE}" 
            } 
        }

    } catch (err) { 
        currentBuild.result = 'FAILURE' 
        throw err 
    } 
}
