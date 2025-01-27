pipeline{
    agent any
    options{
        timeout(time : 1, unit: 'HOURS')
    }
    triggers{
        pollSCM (' * * * * * ')
    }
    tools{
        maven 'MAVEN_3.9.9'
    }
    parameter{
        string(name: 'GOALS', defaultValue:'clean package')
    }
stages{
    stage('SCM'){
        steps{
            git url:'https://github.com/Nikkyreddyk/spring-petclinic-aug24.git',
            branch:'main'

        }  
    }
    stage('BUILD'){
        steps{
            sh "mvn ${params.GOALS}"
        }
    }
}
}