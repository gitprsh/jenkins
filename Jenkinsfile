pipeline{
    agent any

        stages{
            stage('Build'){
                steps{
                    sh 'cd webapp && npm install && npm run build'
                }
            }
            stage('Release'){
                steps{
                    script{
                        def pawankalyan = readJSON file: 'webapp/package.json'
                        def versionnumber = pawankalyan.version
                        sh 'zip lms-${versionnumber}.zip -r dist/*'
                        sh 'curl -v -u admin:surya --uplaod-file lms-${versionnumber}.zip http://18.168.199.36:8081/repository/lms/' 
                    }
                }
            }
            stage('Deploy'){
                steps{
                    script{
                        sh 'rm -rf *.zip'
                        def pawankalyan = readJSON file: 'webapp/package.json'
                        def versionnumber = pawankalyan.version
                        sh 'curl -u admin:surya -X GET http://18.168.199.36:8081/repository/lms/lms-${versionnumber} --output lms.zip'
                        sh 'rm -r dist'
                        sh 'unzip lms.zip'
                        sh 'rm -r /var/www/html/*'
                        sh 'cp -r dist/* /var/www/html/'
                    }
                }
            }
        }
}