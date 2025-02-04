# Using Jenkins to Manage CI/CD Pipelines for Microservices Applications
<img src="images/Jenkins-la-gi.jpg" alt="Mô tả ảnh" width="700"/>


## 1. Install the necessary tools:
1. [**Docker**](https://www.docker.com/): Deploy and run applications in containers.
2. [**Kubectl**](https://kubernetes.io/docs/tasks/tools/): interact with Kubernetes clusters.
3. [**Minikube**](https://minikube.sigs.k8s.io/docs/): Deploy a local Kubernetes cluster, help manage containers, ensure the deployment, scaling, and maintenance of microservices applications automatically and efficiently.
4. [**Jenkin**](https://www.jenkins.io/doc/book/installing/): Automate the process of building, testing and deploying microservices applications to Docker, Kubernetes.

    | **Cài đặt các Plugin cần thiết**               |
    |------------------------------------------------|
    | Plugin Docker Pipeline                         |
    | Kubernetes Plugin                              |
    | Git Plugin                                     |
    | Plugin SonarQube Scanner                       |

5. [**SonarQube**](https://viblo.asia/p/cai-dat-cau-hinh-sonarqube-theo-script-tich-hop-va-tao-webhook-sonarqube-job-tren-jenkins-zOQJwY8NVMP): source code quality check
6. [**Trivy**](https://trivy.dev/v0.18.3/installation/): Open source security scanning, detecting security vulnerabilities in source code, container images. 

## 2.SonarQube Deployment Integration on Jenkins
-	Install SonarQube Scanner Plugin on Jenkins

    <img src="images/image_1.png" alt="Mô tả ảnh" width="500"/>

-	Go to SonarQube to create a login token for Jenkins

    <img src="images/image_2.png" alt="Mô tả ảnh" width="500"/>

-	Configure SonarQube server on Jenkins: 
    -   Vào phần Manage Jenkins => System --> Add Credentials (Thêm token đã tạo ở trên) --> Thêm vào trường Name, URL server và authencation token như dưới hình:

    <img src="images/image_3.png" alt="Mô tả ảnh" width="500"/>

-	Go to sonarqube to create project: will be used in pipeline

    <img src="images/image_4.png" alt="Mô tả ảnh" width="500"/>

## 3.Create Jenkins connection to access Minikube cluster
-	Edit the file: nano ~/.kube/config (stores the information needed to connect to the Kubernetes cluster). Change the path of the certificate files to the encrypted content of the file (do this to avoid the Permission Denied error when Jenkins accesses the Minikube cluster):

    <img src="images/image_5.png" alt="Mô tả ảnh" width="500"/>
    <img src="images/image_6.png" alt="Mô tả ảnh" width="500"/>

Get these values ​​using the following command:

    cat /home/ubuntu/.minikube/ca.crt | base64 -w 0; echo
    cat /home/ubuntu/.minikube/profiles/minikube/client.crt | base64 -w 0; echo
    cat /home/ubuntu/.minikube/profiles/minikube/client.key | base64 -w 0; echo

- Replace the file paths with the following encrypted content:

    <img src="images/image_7.png" alt="Mô tả ảnh" width="500"/>

- Manage Jenkins -> Clouds -> New cloud. Add the above config file to Credentials. Test Connect and see if it works:

    <img src="images/image_9.png" alt="Mô tả ảnh" width="500"/>

## 4. Create pipeline
Use Jenkins to create pipeline.
[Pipeline](Jenkins_file)  This is divided into the following main steps, each of which performs a specific task in the CI/CD pipeline.

| **Pipeline**                                                      |
|-------------------------------------------------------------------|
| Clone source code from GitHub                                     |
| Source Code Security Scanning with Trivy                          |
| Source Code Quality Analysis with SonarQube                       |
| Build Docker images using docker-compose                          |
| Security Scanning of Docker Images with Trivy                     |
| Deploy applications to Kubernetes                                 |
| Check deployment status                                           |
| Report pipeline results (complete, success or failure)            |

- Create pipeline:

    <img src="images/image_16.png" alt="Mô tả ảnh" width="500"/>

- Configure pipeline script:

    <img src="images/image.png" alt="Mô tả ảnh" width="500"/>


## 5. Successful pipeline run result:
- Results in Verify Deployment step:

    <img src="images/image_11.png" alt="Mô tả ảnh" width="500"/>

- State pipeline: 

    <img src="images/image_12.png" alt="Mô tả ảnh" width="500"/>

- Try accessing the front-end of the microservices app:

    <img src="images/image_13.png" alt="Mô tả ảnh" width="500"/>

- Check the generated trivy report files:

    <img src="images/image_14.png" alt="Mô tả ảnh" width="500"/>
    <img src="images/image_15.png" alt="Mô tả ảnh" width="500"/>

