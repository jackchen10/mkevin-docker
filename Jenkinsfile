pipeline {
      agent any

      environment {
        MVN_PATH = '/usr/local/Cellar/maven/3.6.0/libexec/bin/mvn'
        IMG = 'mkevin-docker'
        VER = '0.0.1'
        JAR = 'mkevin-docker-0.0.1-SNAPSHOT'
        SOURCE_DIR = 'usr/local/Cellar/maven/3.6.0/libexec/repo/net/mkevin/mkevin-docker/0.0.1-SNAPSHOT/'
        BUILD_DIR = '/Applications/devtools/docker_project/jenkins_home/docker/build/'
        CLASS_DIR = '/Applications/devtools/docker_project/jenkins_home/workspace/empty/target/classes'
      }

      //阶段
      stages {
        //每一个阶段
		stage('pull git') {
			steps{
			    /*从git拉取代码，可实用工具生成*/
				git credentialsId: 'a4760f62-b4b6-49f1-93f2-4f760613271c', url: 'https://github.com/jackchen10/mkevin-docker.git'
			}

		}
		stage('maven 构建') {
			steps{
			    /*使用mvn构建，使用全路径模式*/
			    sh '${MVN_PATH} install'
			}
		}
		stage('docker 构建') {
            steps{
                /*创建文件夹，用于放入程序jar包和Dockerfile文件*/
                sh 'mkdir -p ${BUILD_DIR}${JAR}'
                /*copy 程序jar包*/
                sh 'cp ${SOURCE_DIR}${JAR}.jar ${BUILD_DIR}'
                /*copy Dockerfile*/
                sh 'cp ${CLASS_DIR}/Dockerfile ${BUILD_DIR}'
                /*Docker登录、镜像构建、push到私服、运行容器*/
                sh '''
                    docker login -u admin -p chenfeng1982 localhost:5000
                    cd ${BUILD_DIR};
                    pwd;
                    docker build -t localhost:5000/${IMG}:${VER} .
                    docker push localhost:5000/${IMG}:${VER}
                    docker logout localhost:5000
                    docker rm -f ${IMG}
                    docker run --name ${IMG} -p 8080:8080 -d 10.1.18.202:5000/${IMG}:${VER}
                '''
            }
        }
        
   }
}
