This project demonstrates a 3-tier microservices architecture for tracking financial transactions. The frontend is a React SPA served via Nginx, the backend is a Node.js/Express REST API, and the database is MySQL 8.0. All services are containerized with Docker and orchestrated using Docker Compose, communicating over a custom internal Docker network. The project covers multi-stage Docker builds, Nginx reverse proxy configuration, service dependency management, and volume-based data persistence — making it a solid reference for learning containerized full-stack application deployment.


What the project is
"This is a 3-tier expense tracking application containerized with Docker, consisting of a React frontend, a Node.js backend API, and a MySQL database."

Architecture

[Browser] → [Frontend: Nginx on port 80]
                    ↓
          [Backend: Node.js on port 8080]
                    ↓
          [MySQL: Database with persistent volume]

All 3 services on an isolated Docker network called "expense"
Best Practices
1. Multi-Stage Build (Backend)

"I used a multi-stage build for the backend. The builder stage installs npm dependencies, and the final image only copies the built output — this cuts image size by ~150MB as I noted in the Dockerfile comments."

2. Alpine Base Images

"Both the backend and frontend use Alpine-based images (node:alpine, nginx:alpine) which are minimal and have a smaller attack surface compared to full Debian images."

3. Non-Root User

"I created a dedicated expense user for the backend and used the nginx user for the frontend — neither container runs as root, which is a security best practice."

4. Layer Caching Optimization

"In the backend Dockerfile, I copy package.json first before the application code. This means Docker only re-runs npm install when dependencies change, not on every code change — making rebuilds faster."

5. Named Volumes for Data Persistence

"MySQL data is stored in a named Docker volume so data survives container restarts and recreation."

6. Custom Isolated Network

"All services communicate over a custom expense network. The frontend and backend are never exposed directly — only port 80 on the frontend is published to the host."

7. Version Tagging

"Images are tagged with 1.0.1 instead of latest, ensuring consistent and reproducible deployments."

8. Service Discovery

"The backend connects to MySQL using the hostname mysql — Docker's built-in DNS resolves container names within the custom network."
