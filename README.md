# OutlineWebhook

This project is modified from [Frando's Outline Webhook example](https://gist.github.com/Frando/aa561ca7e6c72ab64b5d17df911c0b1f). It provides a simple webhook server built with Deno.

## Prerequisites

- A keycloak client configured in your realm with service account permissions to access keycloak user groups
- A webhook configured in outline (note that the container expects the webhook to come to /webhook)

## Running the Application

1. Build the Docker image:
   ```bash
   docker build -t outline-webhook .
   ```
2. Run the Docker container:
   ```bash
   docker run -p 8000:8000 outline-webhook
   ```
3. The server will be accessible on port `8000`.

## Environment Variables

### Core Configuration
- `DENO_ENV`: Set to `production` by default in the Dockerfile.
- `WEBHOOK_SECRET`: Your Outline webhook secret.
- `OUTLINE_ENDPOINT`: The API endpoint of your Outline instance (e.g., `https://your-outline-instance.com/api`).
- `OUTLINE_API_TOKEN`: Your Outline API token.
- `KEYCLOAK_ENDPOINT`: The endpoint of your Keycloak instance (e.g., `https://your-keycloak-instance.com`).
- `KEYCLOAK_REALM`: Your Keycloak realm.
- `KEYCLOAK_CLIENT_ID`: Your Keycloak client ID.
- `KEYCLOAK_CLIENT_SECRET`: Your Keycloak client secret.

### Role Management (Optional)
Configure which Keycloak groups map to which Outline roles. Each variable accepts a comma-separated list of group names:

- `OUTLINE_GUEST_GROUPS`: Keycloak groups that should have "guest" role in Outline (e.g., `sponsors,students`).
- `OUTLINE_VIEWER_GROUPS`: Keycloak groups that should have "viewer" role in Outline.
- `OUTLINE_EDITOR_GROUPS`: Keycloak groups that should have "editor" role in Outline.
- `OUTLINE_ADMIN_GROUPS`: Keycloak groups that should have "admin" role in Outline.

**Role Priority**: If a user belongs to multiple groups mapped to different roles, the highest priority role is assigned: admin > editor > viewer > guest.

**Note**: Users in groups not listed in any role mapping will retain their existing Outline role.

## Credits

This project is inspired by and based on [Frando's Outline Webhook example](https://gist.github.com/Frando/aa561ca7e6c72ab64b5d17df911c0b1f). Special thanks to Frando for the original implementation.