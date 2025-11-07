# `@dify-chat/api`

![version](https://img.shields.io/npm/v/@dify-chat/api) ![NPM Last Update](https://img.shields.io/npm/last-update/@dify-chat/api) ![NPM Downloads](https://img.shields.io/npm/dm/@dify-chat/api)

`@dify-chat/api` is a package in the [Dify Chat](https://github.com/lexmin0412/dify-chat) project that provides a complete set of methods to operate Dify applications, including getting application information, managing conversations, sending messages, and more.

The following will introduce how to integrate and use it in your application.

## Installation

Install via npm/yarn/pnpm:

```bash
# npm
npm install @dify-chat/api

# yarn
yarn add @dify-chat/api

# pnpm
pnpm add @dify-chat/api
```

## Basic Usage

```ts
import { createDifyApiInstance, DifyApi } from '@dify-chat/api';

// Method 1: Create instance using factory function
const api = createDifyApiInstance({
  user: 'user123',
  apiBase: 'https://api.dify.ai/v1',
  apiKey: 'app-YOUR_API_KEY',
});

// Method 2: Direct instantiation
const api2 = new DifyApi({
  user: 'user123',
  apiBase: 'https://api.dify.ai/v1',
  apiKey: 'app-YOUR_API_KEY',
});

// Call API
api.getAppInfo().then((appInfo) => {
  console.log(appInfo);
});
```

## API Instance Configuration

When instantiating `DifyApi`, you need to provide the following configuration:

```ts
interface IDifyApiOptions {
  /**
   * User identifier
   */
  user: string;
  /**
   * API prefix, default https://api.dify.ai/v1
   */
  apiBase: string;
  /**
   * Dify APP API key
   */
  apiKey: string;
}
```

## API Methods

> Note: The original intention of developing this package was to implement the related functionality of the main project Dify Chat, so not all official APIs will have corresponding methods. If you need to call other APIs, please refer to the official documentation.

### Update API Configuration

When you need to switch applications, you can update the API configuration.

```ts
api.updateOptions({
  user: 'newUser',
  apiBase: 'https://api.dify.ai/v1',
  apiKey: 'app-NEW_API_KEY',
});
```

### Get Application Basic Information

```ts
const appInfo = await api.getAppInfo();
```

**Return Type:**

```ts
interface IGetAppInfoResponse {
  name: string;
  description: string;
  tags: string[];
}
```

### Get Application Meta Information

```ts
const appMeta = await api.getAppMeta();
```

**Return Type:**

```ts
interface IGetAppMetaResponse {
  tool_icons: {
    dalle2: string;
    api_tool: {
      background: string;
      content: string;
    };
  };
}
```

### Get Application Parameters

```ts
const appParameters = await api.getAppParameters();
```

**Return Type:**

```ts
interface IGetAppParametersResponse {
  opening_statement?: string;
  user_input_form: IUserInputForm[];
  suggested_questions?: string[];
  suggested_questions_after_answer: {
    enabled: boolean;
  };
  file_upload: {
    enabled: boolean;
    allowed_file_extensions: string[];
    allowed_file_types: IFileType[];
    allowed_file_upload_methods: Array<'remote_url' | 'local_file'>;
    fileUploadConfig: {
      file_size_limit: number;
      batch_count_limit: number;
      image_file_size_limit: number;
      video_file_size_limit: number;
      audio_file_size_limit: number;
      workflow_file_upload_limit: number;
    };
    image: {
      enabled: false;
      number_limits: 3;
      transfer_methods: ['local_file', 'remote_url'];
    };
    number_limits: number;
  };
  text_to_speech: {
    enabled: boolean;
    autoPlay: 'enabled' | 'disabled';
    language: string;
    voice: string;
  };
  speech_to_text: {
    enabled: boolean;
  };
}
```

### Conversation Management

#### Get Conversation List

```ts
const conversations = await api.listConversations({ limit: 20 });
```

**Parameters:**

```ts
interface IListConversationsRequest {
  limit: number;
}
```

**Return Type:**

```ts
interface IGetConversationListResponse {
  data: IConversationItem[];
}

interface IConversationItem {
  created_at: number;
  id: string;
  inputs: Record<string, unknown>;
  introduction: string;
  name: string;
  status: 'normal';
  updated_at: number;
}
```

#### Rename Conversation

```ts
await api.renameConversation({
  conversation_id: 'conversation_id',
  name: 'New Conversation Name',
});

// Auto-generate name
await api.renameConversation({
  conversation_id: 'conversation_id',
  auto_generate: true,
});
```

**Parameters:**

```ts
{
  conversation_id: string;
  name?: string;
  auto_generate?: boolean;
}
```

#### Delete Conversation

```ts
await api.deleteConversation('conversation_id');
```

#### Get Conversation History Messages

```ts
const history = await api.listMessages('conversation_id');
```

**Return Type:**

```ts
interface IListMessagesResponse {
  data: IMessageItem[];
}

interface IMessageItem {
  id: string;
  conversation_id: string;
  inputs: Record<string, string>;
  query: string;
  answer: string;
  message_files: [];
  feedback?: {
    rating: 'like' | 'dislike';
  };
  status: 'normal' | 'error';
  error: string | null;
  agent_thoughts?: IAgentThought[];
  created_at: number;
  retriever_resources?: IRetrieverResource[];
}
```

### Message Related

#### Send Message

```ts
const response = await api.sendMessage({
  conversation_id: 'conversation_id', // Optional, if not provided, a new conversation will be created
  inputs: {
    // Input parameters, in key-value pair format
    param1: 'value1',
  },
  files: [], // Attachments, can be remote URLs or locally uploaded file IDs
  user: 'user123',
  response_mode: 'streaming',
  query: 'Hello, may I ask...',
});
```

**Parameters:**

```ts
{
  conversation_id?: string;
  inputs: Record<string, string>;
  files: IFile[];
  user: string;
  response_mode: 'streaming';
  query: string;
}
```

#### File Type Definition

```ts
export type IFileType = 'document' | 'image' | 'audio' | 'video' | 'custom';

export interface IFileBase {
  type: IFileType;
}

export interface IFileRemote extends IFileBase {
  transfer_method: 'remote_url';
  url?: string;
}

export interface IFileLocal extends IFileBase {
  transfer_method: 'local_file';
  upload_file_id?: string;
}

export type IFile = IFileRemote | IFileLocal;
```

#### Stop Generation

```ts
await api.stopTask('taskId');
```

#### Get Next Round Suggested Questions

```ts
const suggestions = await api.getNextSuggestions({
  message_id: 'message_id',
});
```

**Return Type:**

```ts
{
  data: string[]
}
```

#### Message Feedback

```ts
await api.createMessageFeedback({
  messageId: 'message_id',
  rating: 'like', // 'like' | 'dislike' | null
  content: 'Feedback content',
});
```

**Parameters:**

```ts
{
  messageId: string;
  rating: 'like' | 'dislike' | null;
  content: string;
}
```

### File Operations

#### Upload File

```ts
const fileInfo = await api.uploadFile(file);
```

**Parameters:**

- `file`: Browser File object

**Return Type:**

```ts
interface IUploadFileResponse {
  id: string;
  name: string;
  size: number;
  extension: string;
  mime_type: string;
  created_by: number;
  created_at: number;
}
```

### Speech Related

#### Text to Speech

```ts
// Convert via message ID
const audioResponse = await api.text2Audio({
  message_id: 'message_id',
});

// Convert via text content
const audioResponse2 = await api.text2Audio({
  text: 'Text content to convert',
});
```

**参数：**

```ts
| {
    message_id: string;
  }
| {
    text: string;
  }
```

#### Speech to Text

```ts
const textResponse = await api.audio2Text(audioFile);
```

**Parameters:**

- `audioFile`: Audio file. Supported formats: ['mp3', 'mp4', 'mpeg', 'mpga', 'm4a', 'wav', 'webm'] File size limit: 15MB

**Return Type:**

```ts
interface IAudio2TextResponse {
  text: string;
}
```

### Workflow Related

#### Execute Workflow

```ts
const workflowResponse = await api.runWorkflow({
  inputs: {
    // Input parameters, in key-value pair format
    param1: 'value1',
    param2: [
      /* File array */
    ],
  },
});
```

Parameters:

```ts
{
  inputs: Record<string, IFile[] | unknown>;
}
```

#### Get Workflow Execution Status

```ts
const workflowResult = await api.getWorkflowResult({
  workflow_run_id: 'workflow_run_id',
});
```

Parameters:

```ts
{
  workflow_run_id: string;
}
```

Response Body:

```ts
{
  /** Workflow execution ID */
  id: string;
  /** Associated Workflow ID */
  workflow_id: string;
  /** Execution status running / succeeded / failed / stopped */
  status: string;
  /** Task input content */
  inputs: object;
  /** Task output content */
  outputs: object;
  /** Error reason */
  error: string;
  /** Total steps of task execution */
  total_steps: number;
  /** Total tokens of task execution */
  total_tokens: number;
  /** Task start time */
  created_at: number;
  /** Task end time */
  finished_at: number;
  /** Elapsed time (s) */
  elapsed_time: number;
}
```

### Text Generation Related

#### Execute Text Generation

```ts
const completionResponse = await api.completion({
  inputs: {
    // Input parameters, in key-value pair format
    param1: 'value1',
    param2: [
      /* File array */
    ],
  },
});
```

Parameters:

```ts
{
  inputs: Record<string, IFile[] | unknown>;
}
```

## Complete Event Types

API responses contain various event types. The complete definition is as follows:

```ts
export enum EventEnum {
  MESSAGE = 'message',
  AGENT_MESSAGE = 'agent_message',
  AGENT_THOUGHT = 'agent_thought',
  MESSAGE_FILE = 'message_file',
  MESSAGE_END = 'message_end',
  TTS_MESSAGE = 'tts_message',
  TTS_MESSAGE_END = 'tts_message_end',
  MESSAGE_REPLACE = 'message_replace',
  ERROR = 'error',
  PING = 'ping',
  WORKFLOW_STARTED = 'workflow_started',
  WORKFLOW_FINISHED = 'workflow_finished',
  WORKFLOW_NODE_STARTED = 'node_started',
  WORKFLOW_NODE_FINISHED = 'node_finished',
}
```

## Example: Complete Conversation Flow

```ts
import { createDifyApiInstance } from '@dify-chat/api';

async function chatExample() {
  // 1. Create API instance
  const api = createDifyApiInstance({
    user: 'user123',
    apiBase: 'https://api.dify.ai/v1',
    apiKey: 'app-YOUR_API_KEY',
  });

  // 2. Get application information
  const appInfo = await api.getAppInfo();
  console.log('Application Info:', appInfo);

  // 3. Get application parameters
  const appParams = await api.getAppParameters();
  console.log('Application Parameters:', appParams);

  // 4. Send message
  const messageStream = await api.sendMessage({
    inputs: {},
    files: [],
    user: 'user123',
    response_mode: 'streaming',
    query: 'Hello, please introduce yourself',
  });

  // 5. Handle streaming response
  const reader = messageStream.body.getReader();
  const decoder = new TextDecoder();

  while (true) {
    const { done, value } = await reader.read();
    if (done) break;

    const chunk = decoder.decode(value);
    const lines = chunk.split('\n').filter((line) => line.trim() !== '');

    for (const line of lines) {
      if (line.startsWith('data: ')) {
        const data = JSON.parse(line.substring(6));
        console.log('Received event:', data.event);

        if (data.event === 'message') {
          console.log('AI reply content:', data.answer);
        } else if (data.event === 'error') {
          console.error('Error:', data.message);
        } else if (data.event === 'message_end') {
          console.log('Message ended');
        }
      }
    }
  }
}
```

## Notes

1. All API methods return Promises, which can be handled using async/await or .then()
2. Message sending uses streaming responses, requiring ReadableStream handling
3. API key format is usually `app-XXXXXXXX`, which needs to be obtained from the Dify console
4. User ID can be any string, used to identify user identity, making it easy to distinguish conversations from different users
