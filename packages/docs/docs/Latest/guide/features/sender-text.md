# Pre-fill Message Content via URL Parameters

In some scenarios, we may want to embed a Dify Chat link in a third-party application, and automatically fill predetermined text into the input box after users click the link to enter the conversation.

## Implementation Principle

Dify Chat automatically obtains the `sender_text` parameter from the URL. When the parameter exists, it will decode it and fill it into the input box.

## How to Use

Assuming the base path of the current Dify Chat frontend is `http://localhost:5200/dify-chat/`, we want to automatically fill `hello` into the input box after users enter Chat.

Build the link (specify application ID, pre-fill message content, whether to start a new conversation):

```shell
http://localhost:5200/dify-chat/app/b38e598b-e766-44d5-895a-95415a1b9bfd?sender_text=hello&isNewCvst=1
```

You can embed this link into your application. When users click this link, they will automatically jump to the Dify Chat page, and the input box will automatically fill with `hello`.

![sender_text_in_url](/guide__sender_text_in_url.png)

## Notes

- The value of the `sender_text` parameter will be automatically decoded, so it can contain special characters.
- The `isNewCvst` parameter is used to specify whether to start a new conversation. By default, it is not enabled. If set to `1`, it will start a new conversation.
