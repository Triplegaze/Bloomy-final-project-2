pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'

                git branch: 'main',
                    url: 'https://github.com/Triplegaze/Bloomy-final-project-2.git'
            }
        }

        stage('Build Java Application') {
            steps {
                echo 'Building Java application with Maven...'

                sh '''
                    cd samplejavaapp
                    mvn -B clean package
                '''
            }
        }

        stage('Deploy with Ansible') {
            steps {
                echo 'Deploying application to EC2...'

                sshagent(['ec2-ssh-key']) {
                    sh '''
                        ansible-playbook \
                            -i ansible/inventory \
                            ansible/playbook.yml \
                            --limit your-ec2
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'CI/CD pipeline completed successfully!'
        }

        failure {
            echo 'CI/CD pipeline failed.'
        }
    }
}
