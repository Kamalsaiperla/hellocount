podTemplate(label: 'mypod', containers: [
    containerTemplate(name: 'git', image: 'alpine/git', ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'maven', image: 'maven:3.3.9-jdk-8-alpine', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'dockercompose', image: 'docker/compose', command: 'cat', ttyEnabled: true)
  ],
  volumes: [
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
  ]
  ) {
    node('mypod') {
        stage('Clone repository') {
            container('git') {
                sh 'git clone https://github.com/Kamalsaiperla/hellocount.git'
                sh 'ls'
                container('dockercompose'){
                    dir('hellocount/'){
                        sh 'docker-compose build'
                        sh 'docker tag hellocount_web pkamalsai/hellocount:$BUILD_NUMBER'
                        sh 'docker images'
                        sh 'docker-compose up -d'
                    }
                }
            }
        }
        stage("Push image") {
            container('docker'){
                dir('hellocount/'){
                sh 'docker login --username pkamalsai --password k@malkee3014'
                sh 'docker push pkamalsai/hellocount:$BUILD_NUMBER'
                }
            }
        }
    }
}
