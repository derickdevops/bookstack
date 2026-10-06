## BookStack Docker Deployment (Production Ready with Nginx Reverse Proxy)
This deployment provides a Docker-based setup for running BookStack with:
✅ MySQL database
✅ SMTP support for sending email notifications (e.g., password reset)
✅ Persistent data storage via Docker volumes
✅ Health checks & restart policies for reliability
✅ Nginx reverse proxy for secure and controlled external access

## Project Structure
bookstack/
├── docker-compose.yml # Defines services: BookStack, MySQL, NGINX
├── .env # Environment-specific variables
├── nginx/
│ └── default.conf # Custom NGINX reverse proxy config
└── README.md 

## How to Deploy
1. Clone the Repository
git clone https://git.edusuc.net/WEBFORX/infrastructure-tools.git
cd infrastructure-tools/bookstack

2. Create and Configure .env
Copy the example environment file:

cp .env.example .env

Edit .env and provide your actual database passwords, SMTP settings, domain details, ports, and other environment-specific variables.
Important: Ensure the APP_URL matches your configured domain (e.g., http://bookstack.webforxtechnology.com).

3. Launch the Stack
docker-compose --env-file .env up -d

Docker will start the BookStack, MySQL, and Nginx containers, connect them on a custom Docker network (internal), and apply health checks and restart policies.

4. Access the Application
In your browser, navigate to your configured domain:

http://bookstack.webforxtechnology.com

Note: The Nginx reverse proxy listens on port 80 and forwards traffic to the BookStack container internally.

5. Login Credentials
Use the default login credentials after initial deployment:

Email: admin@admin.com
Password: password

6. Reset Admin Password (Highly Recommended)
For security, reset the admin password:

docker exec -it bookstack_bookstack_1 php artisan bookstack:reset-admin

## Maintenance Commands

# Start all services
docker-compose --env-file .env up -d

# Check container health & status
docker ps

# View logs (e.g., nginx)
docker-compose logs -f nginx

# Restart just one service
docker-compose restart bookstack


## How It Works (Summary)
MySQL Service: Runs with persistent volume storage and includes built-in health checks.

BookStack Service: Uses the official LinuxServer.io Docker image and connects to MySQL using credentials from the .env file.

Nginx Reverse Proxy: Handles all external HTTP traffic, forwards requests to the internal BookStack container, and provides a layer of separation and easier domain management.

Shared Docker Network: All services communicate internally on bookstack-net.

Configuration & Health: Ports, database credentials, SMTP info, application URLs, and health check timings are dynamically loaded from the .env file. Health checks continuously monitor service status. Docker restart policies ensure containers recover from failures automatically.

## Additional Notes
1. You can extend Nginx configuration for HTTPS using Let's Encrypt (optional).
2. It is recommended to restrict direct access to BookStack's exposed port; only Nginx should handle external     traffic.
3. Modify Nginx configuration in the docker-compose.yml or a custom config file as needed.
