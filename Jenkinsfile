node {
    checkout scm

    // deploy env dev
    stage("Build"){
        docker.image('shippingdocker/php-composer:7.4').inside('-u root') {
            sh 'rm composer.lock'
            sh 'composer install'
        }
    }


    // Testing
    docker.image('ubuntu').inside('-u root') {
       sh 'echo "Ini adalah test"'
    }

    // Deploy ke Dev
    docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
       sshagent (credentials: ['ssh-dev']) {
          sh 'mkdir -p ~/.ssh'
          sh 'ssh-keyscan -H "$HOST" > ~/.ssh/known_hosts'
          sh "rsync -rav --delete ./ ubuntu@$HOST:/home/ubuntu/$HOST/ --exclude=.env --exclude=storage --exclude=.git"
       }
   }
}