# Debug Mode

Debug mode is a development and debugging feature provided by Dify Chat, allowing developers to quickly configure API Keys without any code changes or dependency on backend services, directly connecting to the Dify API to debug application functionality.

## 1. Feature Overview

After entering debug mode, you will see the initial interface of the application list, with a debug button in the bottom right corner of the page.

![Initial Interface](/guide__debug_mode_main.png)

Click the "Debug Button" in the bottom right corner of the page to open a drawer. The drawer contains an application configuration editing area where you can enter the Dify API Base URL and API Key.

![Add Application Configuration Drawer - Filled Information](/guide__debug_mode_data_fulfilled.png)

## 2. Use Cases

Debug mode is suitable for the following scenarios:

- **Local Development**: Directly connect to Dify for frontend development without backend proxy services
- **Feature Testing**: Test interface performance under different application configurations
- **Demo Presentation**: Use simulated data for product demonstrations
- **Problem Troubleshooting**: Isolate server issues and focus on frontend logic debugging

## 3. How to Enable

To start debug mode, there are two options. You can choose according to your needs.

### 3.1. Add Parameter Directly in URL

This option is suitable for one-time debugging scenarios, suitable for temporarily troubleshooting problems or debugging application functionality. After debugging is complete, you can exit directly and return to normal access mode.

Enable method: Add the `isDebug=true` parameter to the URL in the browser address bar, for example:

```
http://localhost:5200/dify-chat/?isDebug=true
```

### 3.2. Specify Environment Variable

This option is suitable for scenarios where debug mode needs to be enabled for a long time and does not support exit.

Enable method: In the `packages/react-app/public/env.js` file, set the debug mode switch directly to `'true'` (note it's a string), then run `pnpm --filter dify-chat-app-react build` to rebuild the product for it to take effect.

```js title="packages/react-app/public/env.js"
window.__DIFY_CHAT_ENV__ = {
  PUBLIC_DEBUG_MODE: 'true',
};
```

If you are building based on docker compose, a more convenient way is to change the `PUBLIC_DEBUG_MODE` variable value of the `react-app` service to `true` in `docker-compose.yml`, then restart the container.

```yaml title="docker-compose.yml"
services:
  react-app:
    environment:
      - PUBLIC_DEBUG_MODE=true
```

## 4. Usage Instructions

> Regardless of how you enable debug mode, all related data is saved in LocalStorage and SessionStorage and will not be uploaded to any third-party server. You can use it with confidence.

### 4.1. Operation Steps

1. Click the debug button in the bottom left corner of the page
2. Enter or modify JSON configuration in the configuration editor
3. You can click the "Use Sample Configuration" button to quickly fill in the template
4. Click the "Save Configuration" button to save settings
5. The page will automatically refresh, and the application list will display the configured debug applications

### 4.2. Notes

- Debug mode should only be used in development environments and should not be enabled in production
- Configuration data is saved locally in the browser. Clearing browser data will lose the configuration
- The API key for debug applications needs to be real and valid to conduct conversations normally

### 4.3. Clear Configuration

To clear debug configuration, you can:

1. Click the exit debug button in the configuration editor, or save after clearing the content
2. Or directly delete the `__DC__DEBUG_APPS` key-value pair in the browser developer tools
