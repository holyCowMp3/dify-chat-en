# `@dify-chat/core`

![version](https://img.shields.io/npm/v/@dify-chat/core) ![NPM Last Update](https://img.shields.io/npm/last-update/@dify-chat/core) ![NPM Downloads](https://img.shields.io/npm/dm/@dify-chat/core)

`@dify-chat/core` is the core package of the [Dify Chat](https://github.com/lexmin0412/dify-chat) project. It exposes helpers for providing and consuming global contexts such as app configuration and conversations.

The sections below show how to integrate it into your application.

## Installation

Install via npm, yarn, or pnpm:

```bash
# npm
npm install @dify-chat/core
# yarn
yarn add @dify-chat/core
# pnpm
pnpm add @dify-chat/core
```

## Usage

### Global Context

#### AppContext

`AppContext` stores the current app configuration and exposes helpers to read and update it.

**AppContextProvider**

Wrap the top-level component that needs app switching functionality with `AppContextProvider`:

```tsx
import { AppContextProvider, ICurrentApp } from '@dify-chat/core';
import { createDifyApiInstance } from '@dify-chat/api';
import { generateUuidV4 } from '@dify-chat/helpers'

const YourChatComponent = () => {

  const { user } = useDifyChat();
  const [appList, setAppList] = useState<ICurrentApp[]>([])

  // Implement your logic for fetching the app list
  const getAppList = async () => {
    setAppList([...])
  }

  const [appLoading, setAppLoading] = useState(true)
  const [ currentApp, setCurrentApp ] = useState<ICurrentApp[]>([]);
  const [currentAppId, setCurrentAppId] = useState('')
  const [difyApi] = useState(() => createDifyApiInstance({
    user,
    apiBase: '',
    apiKey: '',
  }))

  // Fetch the active app's configuration
  const getAppInfo = async () => {
    // Update difyApi options first
    difyApi.updateOptions({
      user,
      apiBase: newApp.requestConfig.apiBase,
      apiKey: newApp.requestConfig.apiKey,
    })
    setAppLoading(true)
    // Fetch the latest app info based on the new currentAppId
    const appConfig = appList.find(item => item.id === currentAppId)
    const difyAppInfo = await difyApi.getAppInfo()
    const appParameters = await getAppParameters(difyApi)
    setAppLoading(false)
    setCurrentApp({
      config: {
        id: generateUuidV4(),
        info: difyAppInfo,
        requestConfig: appConfig.requestConfig,
        answerForm: appConfig.answerForm,
      },
      parameters: appParameters,
    })
  }

  // Fetch the app list on mount
  useEffect(() => {
    getAppList()
  }, [])

  // Refresh the app configuration when currentAppId changes
  useEffect(() => {
    updateAppInfo()
  }, [currentAppId])

  return (
    <AppContextProvider
      value={{
        appLoading,
        currentAppId,
        setCurrentAppId,
        currentApp: currentApp,
        setCurrentApp,
      }}
    >
      Your Chat Inner Component
      <Button onClick={() => setCurrentAppId('new-app-id')}>Switch app</Button>
    </AppContextProvider>
  )
}
```

**useAppContext hook**

Use the `useAppContext` hook inside child components to access the app context:

```tsx
import { useAppContext } from '@dify-chat/core'

const YourInnerComponent = () => {
  const { currentApp, currentAppId } = useAppContext()
  console.log(`Current app ID: ${currentAppId}`, `Current app: ${currentApp}`)
}
```

#### Conversation Context

`ConversationContext` exposes conversation-related helpers, including listing conversations, updating the active conversation ID, and reading the current conversation data.

**ConversationContextProvider**

Wrap the top-level component that needs conversation switching with `ConversationContextProvider`:

```tsx
import { ConversationsContextProvider } from '@dify-chat/core';

const YourChatComponent = () => {
  const { user } = useDifyChat();
  const [conversations, setConversations] = useState([])
  const [currentConversationId, setCurrentConversationId] = useState('')

  // Implement your logic for fetching conversations
  const listConversations = async () => {
    setConversations([...])
  }

  useEffect(() => {
    listConversations()
  }, [])

  return (
    <ConversationsContextProvider
      value={{
        conversations,
        setConversations,
        currentConversationId,
        setCurrentConversationId,
      }}
    >
      <YourInnerComponent />
    </ConversationsContextProvider>
  )
}
```

**`useConversationsContext` hook**

Use the `useConversationsContext` hook inside child components to access the conversation context:

```tsx
import { useConversationsContext } from '@dify-chat/core'

const YourInnerComponent = () => {
  const { conversations, currentConversationId } = useConversationsContext()
  console.log(`Current conversation ID: ${currentConversationId}`, `Conversation list: ${conversations}`)
}
```
