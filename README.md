# Notes-App-CICD
<img src="https://github.com/Harsh971/Notes-App-CICD/blob/main/architecture.gif"></img>

## Project Overview:
This project focuses on automating the deployment of a Notes App. The code, stored on GitHub, is cloned, a Docker image is created and pushed to Docker Hub, and then the application is hosted.

## Key Steps:
1. **Code Management:** 
   - Stored the Notes App code on GitHub for version control and collaboration.
2. **Containerization:** 
   - Created a Docker image of the Notes App, ensuring a consistent and reliable environment.
3. **Image Repository:** 
   - Pushed the Docker image to Docker Hub for easy access and deployment.
4. **Deployment:** 
   - Hosted the Notes App using the Docker image.

## Troubleshooting and Learnings:

🔧 **Docker not able to connect Jenkins User:**
   - **Challenge:** When running the Jenkins CI/CD pipeline, it executes as the Jenkins user, which lacks default permission to run Docker commands.
   - **Solution:** Added the Jenkins user to the Docker user group to grant necessary permissions.
   - ```sudo usermod -aG jenkins docker```

🔒 **Security of Credentials:**
   - **Challenge:** Securely managing sensitive credentials and environment variables.
   - **Solution:** Created environment variables to handle sensitive information securely within the pipeline. 


⚙️ **Application Robustness:**
   - **Challenge:** Encountered an "Application Port already occupied" error.
   - **Solution:** Used Docker-Compose settings instead of individual Docker run commands to manage application ports efficiently and avoid conflicts.

📡 **Automated Rebuilds with GitHub Webhooks:**
   - **Challenge:** Ensuring the application rebuilds automatically upon code changes.
   - **Solution:** Implemented GitHub webhooks to trigger Jenkins builds whenever a new commit is pushed to the GitHub repository, ensuring the application is always up-to-date.

## Technologies Used:
- **Version Control:** GitHub
- **CI/CD Tool:** Jenkins
- **Containerization:** Docker
- **Container Orchestration:** Docker Compose
- **Image Repository:** Docker Hub

## Dockerfile : 
```
FROM python:3.9

WORKDIR /app/backend

COPY requirements.txt /app/backend
RUN pip install -r requirements.txt

COPY . /app/backend

EXPOSE 8000

CMD python /app/backend/manage.py runserver 0.0.0.0:8000
```

## Jenkins Pipeline : 
```
pipeline {
    agent any 
    
    stages{
        stage("Clone Code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/LondheShubham153/django-notes-app.git", branch: "main"
            }
        }
        stage("Build"){
            steps {
                echo "Building the image"
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag my-note-app ${env.dockerHubUser}/my-note-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-note-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"                
            }
        }
    }
}
```


## Learnings:
- Ensured smooth and secure deployment processes.
- Improved my understanding of user permissions and environment variable management.
- Enhanced the robustness and reliability of Dockerized applications.
- Gained valuable experience with GitHub webhooks for automated rebuilds.

Code Source : [Click here](https://github.com/LondheShubham153/django-notes-app)<br>
Project Reference : [Click here](https://www.google.com/search?q=shubham+londhe+cicd+declarative+pipeline&rlz=1C1CHBF_enIN1043IN1044&oq=shubham+londhe+cicd+declarative+pipeline&gs_lcrp=EgZjaHJvbWUyCwgAEEUYChg5GKAB0gEJMTY2OTVqMGo5qAIAsAIB&sourceid=chrome&ie=UTF-8#)

## Feedback

Your feedback is valuable! If you have suggestions for improving existing content or ideas for new additions, please open an issue or reach out to the repository maintainers. If you have any other feedbacks, you can reach out to us at harsh.thakkar0369@gmail.com


## Connect with Me
<p>

 <a href="https://twitter.com/HarshThakkar971" target="blank"><img align="center" src="https://img.freepik.com/premium-vector/vector-new-twitter-x-white-logo-black-background_744381-866.jpg" alt="HarshThakkar971" height="40" width="50" /></a>
  &nbsp;&nbsp;
  	<a href="https://linkedin.com/in/harsh-thakkar-7764bb1a4" target="blank"><img align="center" src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/ca/LinkedIn_logo_initials.png/800px-LinkedIn_logo_initials.png" alt="harsh-thakkar-7764bb1a4" height="40" width="40" /></a>
  &nbsp;&nbsp;
 <a href="https://instagram.com/harsh_thakkar09" target="blank"><img align="center" src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e7/Instagram_logo_2016.svg/768px-Instagram_logo_2016.svg.png" alt="harsh_thakkar09" height="40" width="40" /></a>
</p>
