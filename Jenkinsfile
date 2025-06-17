pipeline {
    agent any

    tools {
        dependencyCheck 'OWASP_DC_CLI'
    }

    stages {
        stage('Instalar dependencias') {
            steps {
                bat 'npm install'
            }
        }

        stage('Análisis de Dependencias') {
            steps {
                bat 'if not exist "dependency-check-report" mkdir "dependency-check-report"'
                tool name: 'OWASP_DC_CLI'

                dependencyCheck odcInstallation: 'OWASP_DC_CLI', additionalArguments: '''
                    --project "SafeNotes" ^
                    --scan "." ^
                    --format "HTML" ^
                    --format "XML" ^
                    --out "dependency-check-report" ^
                    --enableExperimental
                '''
            }
        }

        stage('Publicar Reporte') {
            steps {
                publishHTML(target: [
                    reportDir: 'dependency-check-report',
                    reportFiles: 'dependency-check-report.html',
                    reportName: 'OWASP Dependency-Check Report'
                ])
            }
        }
    }
}
