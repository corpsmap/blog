
This is the second in a series of posts describing how I set up my local development environment using Docker to emulate our production environment without having to have access to any server-side resources. [Previously]() I described setting up Apache HTTPD as a local reverse proxy to handle HTTPS and CAC/PIV integration. Next we'll get into how we use Keycloak for authentication/authorization in a local environment.

Keycloak is an open-source