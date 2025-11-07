# Quick Trial

For demonstration purposes, we have deployed a debug mode application on Github Pages. You can visit https://lexmin0412.github.io/dify-chat to access it.

> In debug mode, Dify Chat will not upload any information you enter to the developer's server. All data is cached locally, and the frontend page directly connects to the Dify API. You can safely try it out.

After entering the demo site, you will see the initial interface of the application list, with a debug button in the bottom right corner of the page.

![Initial Interface](/guide__debug_mode_main.png)

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

## Fill in Application Configuration

Click the "Debug Button" in the bottom right corner of the page:

![Debug Mode Button](/guide__debug_mode_button.png)

You can see the debug mode data configuration drawer. Click the "Use Sample Configuration" button below the input box:

![Use Sample Configuration Button](/guide__debug_mode_use_sample_data_button.png)

Fill in your apiBase and apiKey in sequence:

![Add Application Configuration Drawer - Filled Information](/guide__debug_mode_data_fulfilled.png)

Click the "Save Configuration" button below. When prompted "Debug configuration saved successfully", a new entry will appear in the application list:

![Debug Configuration Saved Successfully](/guide__debug_mode_save_success.png)

Click the application card to enter the application details page and start chatting～

![Main Interface](/guide__sample_chat_main.png)
