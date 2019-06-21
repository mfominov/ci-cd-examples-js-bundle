node ('dockerhost') {
    deleteDir()
    stage('repo checkout') {
        git (url: "git@github.com:mfominov/ci-cd-examples-js-bundle.git",
            credentialsId: 'id_rsa_jenkins_gitlab',
            branch: 'master'
        )
    }
    stage('npm publish') {
        env.NPM_EMAIL="lm-sa-devops@leroy-merlin.ru"
        docker.image('docker.art.lmru.tech/node:10-stretch').inside() { c ->
            withNPM(npmrcConfig: 'devops_npmrc_art') {
                withCredentials([usernamePassword(credentialsId: 'lm-sa-devops', usernameVariable: 'NPM_USER', passwordVariable: 'NPM_PASS')]) {
                    sh """git config user.email 'lm-sa-devops@leroy-merlin.ru'
                          git config user.name 'lm-sa-devops'
                          echo -e '\n_auth='\$(echo -n '$NPM_USER:$NPM_PASS' |base64) >> ./.npmrc
                          echo 'always-auth = true' >> ./.npmrc
                          npm publish"""
                }
            }
        }
    }
}
