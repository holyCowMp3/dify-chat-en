# One-Click Startup with Shell Script

If you don't have a Docker environment, you can also start the project from source code. This section will teach you how to complete the project build and startup of Dify Chat with a one-click script.

## 0. Deployment Environment

Before starting deployment, you need to prepare the following environment:

- Node.js >= 20
- Pnpm >= 10.8.1

## 1. Clone Project Source Code

```bash
git clone git@github.com:lexmin0412/dify-chat.git
```

## 2. Configure Environment Variables

Enter the project directory:

```bash
cd dify-chat
```

Configure react-app environment variables:

```bash
cd packages/react-app
cp .env.template .env
```

Configure platform environment variables:

```bash
cd packages/platform
# Note: Replace DATABASE_URL with your own database connection
cp .env.template .env
```

## 3. Run Startup Script

```bash
chmod +x ./prod-start.sh
./prod-start.sh
```

The script will automatically complete dependency installation, source code building, and service startup. After a moment, if you see the following image, it means the execution was successful:

![Startup Success Prompt](/guide__one_key_start.png)

Visit `http://localhost:5300`, and you will see the Platform initialization interface. Enter username, email, and password in sequence to create an administrator account. This account can be used to log in to `Dify Chat Platform`.

For the frontend application, you need to deploy the artifacts in `packages/react-app/dist` through a static file server such as nginx.

## 4. Modify Environment Variables

If you need to modify environment variable values (such as database connection address), you can edit the .env file created in step 2, then run `./prod-start.sh` again.
