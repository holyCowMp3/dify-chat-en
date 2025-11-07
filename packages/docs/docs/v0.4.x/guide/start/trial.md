# 30 Second Trial

For demonstration purposes, we have deployed a multi-app mode React SPA version on Github Pages. You can visit https://lexmin0412.github.io/dify-chat/ to try it out.

Click the above link, and you will see the initial interface of the application list:

![Initial Interface](/apps_init.png)

## Preparation

First, you need to obtain several key variables from the Dify console:

| Variable | Description                                                                                   |
| -------- | --------------------------------------------------------------------------------------------- |
| API Base | Dify API request prefix. If you are using Dify's official cloud service, it is `https://api.dify.ai/v1` |
| Api Key  | Dify API key used to access the corresponding application's API. Dify applications and API keys have a one-to-many relationship |

Enter the Dify application details and click `Access API` on the left:

![Get Domain and Prefix](/get_api_base.png)

The domain displayed after `API Server` is the value of the `API Base` variable.

Click the `API Key` button on the right to see the API Key management popup:

![Get API Key](/get_api_key_entry.png)

You can choose to create a new API Key or copy an existing API Key.

![Get API Key](/get_api_key.png)

After completing the above steps, we will get the following information:

- API Base: `https://api.dify.ai/v1` OR `${SELF_HOSTED_API_DOMAIN}/v1`
- API Key: `app-YOUR_API_KEY`

## Add Application Configuration

Click the "Add Application Configuration" button at the bottom of the page:

![Add Application Configuration Button](/guide_mtapp_setting.png)

Fill in the application information in sequence:

- Request Configuration: API Base and API Secret obtained in the previous step
- Application Type: Default is Chat Assistant. If it's another type of application, switch to the corresponding type
- Other configurations are optional. Keep the default values first, and edit them later if needed

![Add Application Configuration Drawer - Filled Information](/guide_mtapp_setting_add_fulfilled.png)

Click the OK button. When prompted "Configuration added successfully", a new entry will appear in the application list:

![Add Application Configuration Success](/guide_mtapp_setting_add_success.png)

At this point, you can click the "More" icon in the upper right corner of the application card to edit and delete the application:

![Application Card Actions](/guide_mtapp_app_actions.png)

Click the application card to enter the application details page and start chatting～

![Main Interface](/guide_mtapp_main.png)
