# Docker Compose One-Click Deployment

When using Docker Compose, we provide two methods for deployment. You can choose according to your needs.

## Direct Deployment

This method is suitable for scenarios that don't require code modification. You don't need to clone the source code, just use the official image for deployment.

### 1. Prepare Working Directory

```bash
mkdir dify-chat && cd dify-chat
```

### 2. Download Configuration File

```bash
curl -O https://raw.githubusercontent.com/lexmin0412/dify-chat/main/docker-compose.yml
```

### 3. Modify Configuration

```bash
# Edit configuration file, need to configure DATABASE_URL as the actual database connection address (MySQL)
nano docker-compose.yml
```

### 4. Start Container

```bash
docker-compose -f docker-compose.yml up -d
```

### 5. Access Application

> serverip is your server IP. If starting locally, you can directly use localhost to access

- React App: http://serverip:5200
- Platform API: http://serverip:5300

## Build Image After Code Modification

If you need to modify Dify Chat, you need to clone the source code and build the image yourself.

### 1. Clone Code Repository

```bash
git clone git@github.com:lexmin0412/dify-chat.git
```

### 2. Configure Local Environment Variables

Copy react-app environment variable configuration file:

```bash
cd packages/react-app
cp .env.template .env
```

Copy platform environment variable configuration file:

```bash
cd packages/platform
cp .env.template .env
```

Note: By default, Dify Chat uses MySQL for persistent storage of application configuration. If you need to configure other types of databases, please refer to [Using Other Databases](/guide/deploy/db-config#2-using-other-databases).

### 3. Modify Source Code

Modify the code and test it yourself.

### 4. Build Image Based on Local Code and Start

For code modification scenarios, we have prepared a dedicated docker compose configuration file. You can use it directly. It will read the .env file under the corresponding sub-package as environment variables to start the container.

```bash
docker-compose -f docker-compose.dev.yml up -d
```
