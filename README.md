# Final_devops

## Project Overview

This project is a simple static web application served by Nginx in Docker.

The repository includes:
- `Dockerfile` – builds the image from `nginx:alpine`, copies Nginx config and static site files.
- `docker-compose.yml` – defines a service named `web` with port mapping and restart policy.
- `nginx/default.conf` – Nginx configuration for serving the static site and caching assets.
- `index.htm` – the static landing page.

---

## Development Part

### What is included
- `index.htm`: the static HTML page displayed to users.
- `nginx/default.conf`: the Nginx configuration file used inside the container.
- `Dockerfile`: builds the Docker image and copies the static site into the Nginx web root.
- `docker-compose.yml`: defines how to run the app locally with Docker Compose.

### Local development workflow
1. Open the project in your code editor.
2. Modify `index.htm` or add additional static assets.
3. Rebuild the Docker image and start the service:
   ```powershell
   docker compose up --build
   ```
4. Visit `http://localhost:8080` in your browser.

### Notes
- The site root is `/usr/share/nginx/html` inside the container.
- Static assets are served directly by Nginx.
- The Nginx config uses `try_files` to fallback to `index.html` for front-end routing.

---

## Operation Part

### Running in production-like mode
Use Docker Compose to run the app in detached mode:
```powershell
docker compose up -d --build
```

### Service behavior
- The service listens on port `80` inside the container.
- It is mapped to port `8080` on the host via `docker-compose.yml`.
- The Compose service uses `restart: unless-stopped` so it restarts automatically if the container exits unexpectedly.

### Monitoring and maintenance
- View logs:
  ```powershell
  docker compose logs -f
  ```
- Stop the service:
  ```powershell
  docker compose down
  ```
- Redeploy after changes:
  ```powershell
  docker compose up -d --build
  ```

### Deployment considerations
- For production, run behind a load balancer or reverse proxy if needed.
- Keep the static files and Nginx config under version control.
- Update the Nginx configuration in `nginx/default.conf` for caching, gzip, or security headers if required.
- Use a container orchestration platform or CI/CD pipeline for automated deployments.
