# Building and Deploying a Dockerized User Profile App
Welcome to my project documentation, where I will guide you through the process of building and deploying a user profile application using Docker. By containerizing each component of the app, we ensure consistency and portability, making it easier to deploy in various environments. This documentation serves as a comprehensive guide for Junior DevOps Engineers, showcasing their skills in Docker and containerization.

## Introduction
In my quest to learn DevOps, I have been exploring Docker, a powerful tool for containerization. Containers provide an isolated and consistent runtime environment for applications, allowing for easier deployment and scalability. Throughout this project, I have leveraged Docker to set up a user profile app that consists of a front-end, a Node.js backend, and a MongoDB database. All components are containerized, providing a streamlined and reproducible development environment.


## Project Setup
To begin, I utilized a project spearheaded by [Nana Janashia](https://www.linkedin.com/in/nana-janashia/?originalSubdomain=at) available on [GitLab](https://gitlab.com/nanuchi/developing-with-docker). The project showcases a simple user profile app built with pure JavaScript, CSS styles, a Node.js backend using the Express module, and MongoDB for data storage. The entire application is container-based, ensuring consistency and ease of deployment.

## Docker Basics
Before diving into the project implementation, it's important to understand some Docker basics. Docker uses images and containers as its core concepts:

1. **Image**: A lightweight, standalone, and executable software package that includes everything needed to run a piece of software, such as code, runtime, libraries, environment variables, and configuration files.

2. **Container**: An instance of an image that runs as a process on the host operating system. Containers are isolated from each other and provide a consistent runtime environment for applications.

