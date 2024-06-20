```markdown
# Local Caddy Setup with Docker and Automatic HTTPS

This guide will help you set up a local environment using Docker and Caddy with automatic HTTPS using Caddy's internal CA. This setup includes two backend services and a Caddy reverse proxy.

## Project Directory Structure

Create a directory for your project:

```bash
mkdir caddy-local-setup
cd caddy-local-setup
```

```
caddy-local-setup/
│
├── backend1/
│   ├── Dockerfile
│   └── index.html
│
├── backend2/
│   ├── Dockerfile
│   └── index.html
│
├── Caddyfile
└── docker-compose.yml
```

Add the following entries to your hosts file:

```
127.0.0.1 yoorclinic.local
127.0.0.1 demo-clinic.local
```

## How to Run the Project

Follow these steps to build and run the project:

### Build and Run Containers

Rebuild the Docker images and start the containers:

```bash
docker-compose up --build
```

### Trust Caddy's Local CA Certificate

To avoid certificate warnings, you need to trust Caddy's local CA certificate. Export the CA certificate from the running Caddy container:

```bash
docker cp $(docker-compose ps -q caddy):/data/caddy/pki/authorities/local/root.crt .
```

Trust the `root.crt` file on your local machine. This process varies by operating system:

#### On macOS:

```bash
sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain root.crt
```

#### On Windows:

1. Open `certlm.msc`
2. Right-click on "Trusted Root Certification Authorities" > "All Tasks" > "Import"
3. Follow the wizard to import the `root.crt`.

#### On Linux:

1. Copy `root.crt` to `/usr/local/share/ca-certificates/`
2. Update CA certificates:

```bash
sudo cp root.crt /usr/local/share/ca-certificates/root.crt
sudo update-ca-certificates
```

### Test the Setup

Open your web browser and navigate to [https://yoorclinic.local](https://yoorclinic.local) and [https://demo-clinic.local](https://demo-clinic.local). You should see the content served by the backend servers with a secure connection.

To verify the setup using `curl`, you can use:

```bash
curl -v https://yoorclinic.local
```

## Summary

- **Project Setup**: Create directories and files for backend servers and Caddy configuration.
- **Backend Servers**: Use Docker to serve simple HTML files.
- **Caddy Configuration**: Configure Caddy for reverse proxy and automatic HTTPS.
- **Docker Compose**: Define services and dependencies.
- **Modify Hosts File**: Map local domains to localhost.
- **Build and Run**: Use Docker Compose to start the environment.
- **Trust CA Certificate**: Avoid certificate warnings by trusting the local CA.
- **Test**: Access local domains to verify the setup.
```

This revised markdown properly structures the content and adds necessary formatting for better readability and usability.
