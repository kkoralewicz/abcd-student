pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage('Code checkout from GitHub') {
            steps {
                script {
                    cleanWs()
                    git credentialsId: 'github-token', url: 'https://github.com/kkoralewicz/abcd-student.git', branch: 'main'
                }
            }
        }
        stage('Example') {
            steps {
                echo 'Hello!'
                sh 'ls -la'
                sh 'ls -l ${WORKSPACE}/.zap'
            }
        }

        // ZAD 1
        /*stage('[ZAP] Baseline passive-scan') {
            steps {
                sh 'mkdir -p results/'
                sh '''
                    docker run --name juice-shop -d --rm \
                        -p 3000:3000 \
                        bkimminich/juice-shop
                    sleep 5
                '''
                sh '''
                    docker run --name zap \
                        --add-host=host.docker.internal:host-gateway \
                        -v ${WORKSPACE}/.zap:/zap/wrk/:rw \
                        -t ghcr.io/zaproxy/zaproxy:stable bash -c \
                        "ls -l /zap/wrk/ ; zap.sh -cmd -addonupdate; zap.sh -cmd -addoninstall communityScripts -addoninstall pscanrulesAlpha -addoninstall pscanrulesBeta -autorun /zap/wrk/passive.yaml/passive.yaml" \
                        || true
                '''
            }
            post {
                always {
                    sh '''
                        docker cp zap:/zap/zap_html_report.html ${WORKSPACE}/results/zap_html_report.html
                        docker cp zap:/zap/zap_xml_report.xml ${WORKSPACE}/results/zap_xml_report.xml
                        docker stop zap juice-shop
                        docker rm zap
                    '''
                }
            }
            } */

        
        // ZAD 2
        stage('[OSV-SCANNER] Baseline scan') {
            steps {
                sh '''
                    docker run --name osv-scanner \
                        -v ${WORKSPACE}:/workspace osv-scanner \
                        "osv-scanner scan --lockfile package-lock.json"
                        || true
                '''
            }
            }
        }
}
