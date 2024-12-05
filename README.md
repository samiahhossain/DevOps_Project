## Multi-Container Docker Application with CI/CD: Calculator App Project

#### Complete Project Instructions: [DevOps Foundations Course/Project](https://github.com/shiftkey-labs/DevOps-Foundations-Course/tree/master/Project)

#### Submission by - **Samiah Hossain**

### Project Overview

- **Brief project description:** What is the purpose of your application?

A calculator app built with React. It performs basic arithmetic
and includes additional operations like logarithms and square roots. 
Users interact with it via a web interface.


- **Which files are you implementing? and why?:**

- Dockerfile (frontend): makes the frontend portable and consistent across environments
- requirements.txt: lists python dependencies
- Dockerfile (backend): consistent deployent for backend
- github-actions.yml: pipeline for automated testing/building/deployment
- docker-compose.yml: multi-container orchestration

- _**Any other explanations for personal note taking.**_

None.


### Docker Implementation

**Explain your Dockerfiles:**

- **Backend Dockerfile** (Python API):
    - Here please explain the `Dockerfile` created for the Python Backend API. 
    - This can be a simple explanation which serves as a reference guide, or revision to you when read back the readme in future. 

- FROM python:3.9-slim: start with python image
- WORKDIR /app: sets /app as the working directory inside the container
- COPY . .: copies everything from current directory to /app in the container
- RUN pip install -r requirements.txt: installs packages listed in txt file
- EXPOSE 5000: app listens on port 5000 inside container
- ENV FLASK_ENV=production: run in production mode
- CMD ["python", "app.py"]: start when container is launched

- **Frontend Dockerfile** (React App):
    - Similar to the above section, please explain the Dockerfile created for the React Frontend Web Application. 

- FROM node:14-alpine: start with Alpine image
- WORKDIR /app: sets /app as the working directory inside the container
- COPY ./package.json ./package-lock.json ./: copy these files into container
- RUN npm install: install all dependencies in pachage.json
- COPY . .: copies everything from current directory to /app in the container
- RUN npm run build: run react build script for production
- EXPOSE 3000: app listens on port 3000 inside container
- ENV FLASK_ENV=production: run in production mode
- CMD ["npm", "start"]: start when container is launched

**Use this section to document your choices and steps for building the Docker images.**


### Docker Compose YAML Configuration

**Break down your `docker-compose.yml` file:**

- **Services:** List the services defined. What do they represent?
- **Networking:** How do the services communicate with each other?
- **Volumes:** Did you use any volume mounts for persistent data?
- **Environment Variables:** Did you define any environment variables for configuration? 

**Use this section to explain how your services interact and are configured within `docker-compose.yml`.**

- Two services: frontend (user-facing interface) and backend (the operations)
- They communicate using Docker's default bridge network.
- I did use volume mounts. I mounted the local folders to /app for live code updates, and mounted /app/node-modules to prevent conflicts
- I did define an environment variable: the REACT_APP_BACKEND_URL, which
configures the frontend to communicate with the backend.


### CI/CD Pipeline (YAML Configuration)

**Explain your CI/CD pipeline:**

- What triggers the pipeline (e.g., push to main branch)?
- What are the different stages (build, test, deploy)?
- How are Docker images built and pushed to a registry (if applicable)?

**Use this section to document your automated build and deployment process.**

- Push to main branch or pull request to main branch triggers the pipeline
- Build and test stages are consisted of checkout code, setup node.js/python, install dependencies. Frontend also includes build react app. Backend also includes run tests.
- Docker images are built after successful frontend and backend jobs. Then secrets help authenticate for Docker Hub, and the frontend and backend inages are pushed.
- Deploy stage is included but only contains a placeholder in place of actual deployment.


### Assumptions

- List any assumptions you made while creating the Dockerfiles, `docker-compose.yml`, or CI/CD pipeline. 

- Frontend test always failed so commented it out


### Lessons Learned

- What challenges did you encounter while working with Docker and CI/CD?
- What did you learn about containerization and automation?

**Use this section to reflect on your experience and learnings when implementing this project.**

Challenges:
- Understanding what needs to be included in a docker-compose file
- Getting the frontend container running (had issues with react-scripts)
- Getting my CI/CD pipeline to pass (frontend tests)

I learned that automating the building and testing process drastically reduces the time taken for those tasks, as commands don't have to given one by one and be put into one file. I learned that containerization greatly simplifies deployment through packages an application with its dependencies, and it makes development a lot more efficient.

### Future Improvements

- How could you improve your Dockerfiles, `docker-compose.yml`, or CI/CD pipeline? 
- (Optional-Just for personal reflection) Are there any additional functionalities you would like to consider for the calculator application to crate more stages in the CI/CD pipeline or add additional configuration in the Dockerfiles?

**Use this section to brainstorm ways to enhance your project.**

- Healthchecks could be added to ensure the container application is running correctly.
- Separate environments can be made for development, staging, and production.

<!-- BEST OF LUCK! -->
