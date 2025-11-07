# One-Click Startup

This section will teach you how to complete everything with one click through a script.

Clone project source code:

```bash
git clone git@github.com:lexmin0412/dify-chat.git
```

Enter project directory:

```bash
cd dify-chat
```

Run one-click startup script:

```bash
chmod +x ./prod-start.sh
./prod-start.sh
```

The script will automatically complete dependency installation, database initialization, and project startup. After a moment, if you see the following image, it means the service started successfully:

![Startup Success Prompt](/guide__one_key_start.png)

Visit `http://localhost:5300` to see the `Platform` application login page:

![Platform Login](/guide__platform_login.png)

Go back to the project directory and run `pnpm create-admin`. Follow the prompts to enter email, password, and username in sequence to create an administrator account. This account can be used to log in to `Dify Chat Platform`.

For the frontend application, you need to deploy the artifacts in `packages/react-app/dist` through a static file server such as nginx.

Note: The default port for the Platform application is `5300`. Do not change it arbitrarily.
