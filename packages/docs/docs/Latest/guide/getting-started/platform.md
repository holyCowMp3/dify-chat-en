# Platform Introduction

Dify Chat Platform is a new sub-package added in v0.5.0. It is an application platform based on Dify that provides the following features:

- CRUD operations for application configuration (supports persistent storage)
- Application configuration API accessible to clients
- Dify API proxy service. Clients can interact with Platform through an app_id, avoiding the risk of Dify API Key leakage

After logging in with an administrator account, you can see the following interface:

![Platform Homepage](/guide__platform_app_init.png)

Click the Add button, fill in the API Base and Key to add an application:

![Add Application](/guide__platform_add_app.png)

After clicking OK, you can see the newly added application in the application list:

![Application List](/guide__platform_add_app_success.png)

Go back to the frontend application and refresh the page, and you will see the newly added application.
