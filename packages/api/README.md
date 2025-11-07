# @dify-chat/api

![version](https://img.shields.io/npm/v/@dify-chat/api) ![NPM Last Update](https://img.shields.io/npm/last-update/@dify-chat/api) ![NPM Downloads](https://img.shields.io/npm/dm/@dify-chat/api)

`@dify-chat/api` is a package inside the [Dify Chat](https://github.com/lexmin0412/dify-chat) project. It offers a comprehensive set of methods for working with Dify apps, including fetching app details, managing conversations, and sending messages.

The sections below explain how to integrate and use it in your own application.

## Installation

Install via npm, yarn, or pnpm:

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
import { createDifyApiInstance, DifyApi } from '@dify-chat/api'

// Option 1: create an instance via the factory function
const api = createDifyApiInstance({
  user: 'user123',
  apiBase: 'https://api.dify.ai/v1',
  apiKey: 'app-YOUR_API_KEY',
})

// Option 2: instantiate the class directly
const api2 = new DifyApi({
  user: 'user123',
  apiBase: 'https://api.dify.ai/v1',
  apiKey: 'app-YOUR_API_KEY',
})

// Call the API
api.getAppInfo().then(appInfo => {
  console.log(appInfo)
})
```

## API Instance Options

Provide the following options when instantiating `DifyApi`:

```ts
interface IDifyApiOptions {
    /**
     * User identifier
     */
  user: string
    /**
     * API base URL, defaults to https://api.dify.ai/v1
     */
  apiBase: string
    /**
     * Dify app API key
     */
  apiKey: string
}
```

## API Methods

> Note: This package was created to support the primary Dify Chat project, so it does not wrap every official API. Refer to the official documentation if you need endpoints that are not covered here.

### Update API Options

Update the API configuration when you need to switch applications.

```ts
api.updateOptions({
  user: 'newUser',
  apiBase: 'https://api.dify.ai/v1',
  apiKey: 'app-NEW_API_KEY',
})
```

### Get Basic App Info

```ts
const appInfo = await api.getAppInfo()
```

**Return type:**

```ts
interface IGetAppInfoResponse {
  name: string
  description: string
  tags: string[]
}
```

### Get App Meta

```ts
const appMeta = await api.getAppMeta()
```

**Return type:**

```ts
interface IGetAppMetaResponse {
  tool_icons: {
    dalle2: string
    api_tool: {
      background: string
      content: string
    }
  }
}
```

### Get App Parameters

```ts
const appParameters = await api.getAppParameters()
```

**Return type:**

```ts
interface IGetAppParametersResponse {
  opening_statement?: string
  user_input_form: IUserInputForm[]
  suggested_questions?: string[]
  suggested_questions_after_answer: {
    enabled: boolean
  }
  file_upload: {
    enabled: boolean
    allowed_file_extensions: string[]
    allowed_file_types: IFileType[]
    allowed_file_upload_methods: Array<'remote_url' | 'local_file'>
    fileUploadConfig: {
      file_size_limit: number
      batch_count_limit: number
      image_file_size_limit: number
      video_file_size_limit: number
      audio_file_size_limit: number
      workflow_file_upload_limit: number
    }
    image: {
      enabled: false
      number_limits: 3
      transfer_methods: ['local_file', 'remote_url']
    }
    number_limits: number
  }
  text_to_speech: {
    enabled: boolean
    autoPlay: 'enabled' | 'disabled'
    language: string
    voice: string
  }
  speech_to_text: {
    enabled: boolean
  }
}
```

### Conversation Management

#### List Conversations

```ts
const conversations = await api.listConversations({ limit: 20 })
```

**Parameters:**

```ts
interface IListConversationsRequest {
  limit: number
}
```

**Return type:**

```ts
interface IGetConversationListResponse {
  data: IConversationItem[]
}

interface IConversationItem {
  created_at: number
  id: string
  inputs: Record<string, unknown>
  introduction: string
  name: string
  status: 'normal'
  updated_at: number
}
```

#### Rename a Conversation

```ts
await api.renameConversation({
  conversation_id: 'conversation_id',
  name: 'New conversation name',
})

// Auto-generate the name
await api.renameConversation({
  conversation_id: 'conversation_id',
  auto_generate: true,
})
```

**Parameters:**

```ts
{
  conversation_id: string;
  name?: string;
  auto_generate?: boolean;
}
```

#### Delete a Conversation

```ts
await api.deleteConversation('conversation_id')
```

#### Get Conversation History

```ts
const history = await api.listMessages('conversation_id')
```

**Return type:**

```ts
interface IListMessagesResponse {
  data: IMessageItem[]
}

interface IMessageItem {
  id: string
  conversation_id: string
  inputs: Record<string, string>
  query: string
  answer: string
  message_files: []
  feedback?: {
    rating: 'like' | 'dislike'
  }
  status: 'normal' | 'error'
  error: string | null
  agent_thoughts?: IAgentThought[]
  created_at: number
  retriever_resources?: IRetrieverResource[]
}
```

### Messages

#### Send a Message

```ts
const response = await api.sendMessage({
  conversation_id: 'conversation_id', // Optional; omit to create a new conversation
  inputs: {
    // Input parameters as key-value pairs
    param1: 'value1',
  },
  files: [], // Attachments, either remote URLs or IDs of locally uploaded files
  user: 'user123',
  response_mode: 'streaming',
  query: 'Hello, can you...',
})
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

#### File Type Definitions

```ts
export type IFileType = 'document' | 'image' | 'audio' | 'video' | 'custom'

export interface IFileBase {
  type: IFileType
}

export interface IFileRemote extends IFileBase {
  transfer_method: 'remote_url'
  url?: string
}

export interface IFileLocal extends IFileBase {
  transfer_method: 'local_file'
  upload_file_id?: string
}

export type IFile = IFileRemote | IFileLocal
```

#### Stop Generation

```ts
await api.stopTask('taskId')
```

#### Fetch Suggested Follow-up Questions

```ts
const suggestions = await api.getNextSuggestions({
  message_id: 'message_id',
})
```

**Return type:**

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
  content: 'Feedback message',
})
```

**Parameters:**

```ts
{
  messageId: string
  rating: 'like' | 'dislike' | null
  content: string
}
```

### File Operations

#### Upload a File

```ts
const fileInfo = await api.uploadFile(file)
```

**Parameters:**

- `file`: Browser `File` object

**Return type:**

```ts
interface IUploadFileResponse {
  id: string
  name: string
  size: number
  extension: string
  mime_type: string
  created_by: number
  created_at: number
}
```

### Voice Features

#### Text to Speech

```ts
// Convert by message ID
const audioResponse = await api.text2Audio({
  message_id: 'message_id',
})

// Convert raw text
const audioResponse2 = await api.text2Audio({
  text: 'Text to convert',
})
```

**Parameters:**

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
const textResponse = await api.audio2Text(audioFile)
```

**Parameters:**

- `audioFile`: Audio file. Supported formats: `['mp3', 'mp4', 'mpeg', 'mpga', 'm4a', 'wav', 'webm']`. File size limit: 15 MB.

**Return type:**

```ts
interface IAudio2TextResponse {
  text: string
}
```

### Workflow

#### Run a Workflow

```ts
const workflowResponse = await api.runWorkflow({
  inputs: {
    // Input parameters as key-value pairs
    param1: 'value1',
    param2: [
      /* Array of files */
    ],
  },
})
```

Parameters:

```ts
{
  inputs: Record<string, IFile[] | unknown>
}
```

#### Get Workflow Result

```ts
const workflowResult = await api.getWorkflowResult({
  workflow_run_id: 'workflow_run_id',
})
```

Parameters:

```ts
{
  workflow_run_id: string
}
```

Response body:

```ts
{
  /** Workflow run ID */
  id: string
  /** Associated workflow ID */
  workflow_id: string
  /** Execution status: running / succeeded / failed / stopped */
  status: string
  /** Task input payload */
  inputs: object
  /** Task output payload */
  outputs: object
  /** Error message */
  error: string
  /** Total steps executed */
  total_steps: number
  /** Total tokens consumed */
  total_tokens: number
  /** Start timestamp */
  created_at: number
  /** Finish timestamp */
  finished_at: number
  /** Duration in seconds */
  elapsed_time: number
}
```

### Text Generation

#### Run a Completion

```ts
const completionResponse = await api.completion({
  inputs: {
    // Input parameters as key-value pairs
    param1: 'value1',
    param2: [
      /* Array of files */
    ],
  },
})
```

Parameters:

```ts
{
  inputs: Record<string, IFile[] | unknown>
}
```

## Event Types

API responses contain several event types. The full definition is below:

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

## Example: Full Conversation Flow

```ts
import { createDifyApiInstance } from '@dify-chat/api'

async function chatExample() {
  // 1. Create the API instance
  const api = createDifyApiInstance({
    user: 'user123',
    apiBase: 'https://api.dify.ai/v1',
    apiKey: 'app-YOUR_API_KEY',
  })

  // 2. Fetch app info
  const appInfo = await api.getAppInfo()
  console.log('App info:', appInfo)

  // 3. Fetch app parameters
  const appParams = await api.getAppParameters()
  console.log('App parameters:', appParams)

  // 4. Send a message
  const messageStream = await api.sendMessage({
    inputs: {},
    files: [],
    user: 'user123',
    response_mode: 'streaming',
    query: 'Hi, please introduce yourself',
  })

  // 5. Handle the streaming response
  const reader = messageStream.body.getReader()
  const decoder = new TextDecoder()

  while (true) {
    const { done, value } = await reader.read()
    if (done) break

    const chunk = decoder.decode(value)
    const lines = chunk.split('\n').filter(line => line.trim() !== '')

    for (const line of lines) {
      if (line.startsWith('data: ')) {
        const data = JSON.parse(line.substring(6))
        console.log('Received event:', data.event)

        if (data.event === 'message') {
          console.log('AI reply:', data.answer)
        } else if (data.event === 'error') {
          console.error('Error:', data.message)
        } else if (data.event === 'message_end') {
          console.log('Message end')
        }
      }
    }
  }
}
```

## Notes

1. Every API method returns a Promise; handle them with async/await or `.then()`.
2. Message sending uses streaming responses, so you need to consume the `ReadableStream`.
3. API keys typically look like `app-XXXXXXXX` and must be generated in the Dify console.
4. The `user` ID can be any string that helps you distinguish between different end users.
