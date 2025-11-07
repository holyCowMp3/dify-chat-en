# `@dify-chat/core`

![version](https://img.shields.io/npm/v/@dify-chat/core) ![NPM Last Update](https://img.shields.io/npm/last-update/@dify-chat/core) ![NPM Downloads](https://img.shields.io/npm/dm/@dify-chat/core)

`@dify-chat/core` is the core package in the [Dify Chat](https://github.com/lexmin0412/dify-chat) project, providing global context injection and retrieval functionality for applications, conversations, etc.

The following will introduce how to integrate and use it in your application.

## Installation

Install via npm/yarn/pnpm:

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

`AppContext` is the application context, providing functionality to get and update the current application configuration.

**AppContextProvider**

Use `AppContextProvider` in the top-level component with application switching functionality to provide application context:

```tsx
import { AppContextProvider, ICurrentApp } from '@dify-chat/core';
import { createDifyApiInstance } from '@dify-chat/api';
import { generateUuidV4 } from '@dify-chat/helpers'

const YourChatComponent = () => {

  const { user } = useDifyChat();
  const [appList, setAppList] = useState<ICurrentApp[]>([])

  // Implement logic to get application list
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

  // Define function to get application parameters
  const getAppInfo = async() => {
    // First update difyApi parameters
    difyApi.updateOptions({
      user,
      apiBase: newApp.requestConfig.apiBase,
      apiKey: newApp.requestConfig.apiKey,
    })
    setAppLoading(true)
    // Get new application information based on new currentAppId
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

  // Get application list on initialization
  useEffect(()=>{
    getAppList()
  }, [])

  // Listen to currentAppId changes, update current application configuration
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
      <Button onClick={()=>setCurrentAppId('new-app-id')}>Switch Application</Button>
    </>
  )
}
```

**useAppContext hook**

Use the `useAppContext` hook in your child components to get application context:

```tsx
import { useAppContext } from '@dify-chat/core';

const YourInnerComponent = () => {
  const { currentApp, currentAppId } = useAppContext();
  console.log(`Current Application ID: ${currentAppId}`, `Current Application: ${currentApp}`);
};
```

#### Conversation Context

`ConversationContext` is the conversation context, providing conversation-related functionality, including getting/updating conversation lists, getting/updating current conversation ID, getting current conversation information, etc.

**ConversationContextProvider**

Use `ConversationContextProvider` in the top-level component with conversation switching functionality to provide conversation context:

```tsx
import { ConversationsContextProvider } from '@dify-chat/core';

const YourChatComponent = () => {
  const { user } = useDifyChat();
  const [conversations, setConversations] = useState([])
  const [currentConversationId, setCurrentConversationId] = useState('')

  // Implement logic to get conversation list
  const listConversations = async () => {
    setConversations([...])
  }

  useEffect(()=>{
    listConversations()
  }, [])

  return (
    <ConversationsContextProvider value={{
      conversations,
      setConversations,
      currentConversationId,
      setCurrentConversationId,
    }}>
      <YourInnerComponent />
    </ConversationsContextProvider>
  )
}
```

**`useConversationsContext` hook**

Use the `useConversationsContext` hook in your child components to get conversation context:

```tsx
import { useConversationsContext } from '@dify-chat/core';

const YourInnerComponent = () => {
  const { conversations, currentConversationId } = useConversationsContext();
  console.log(
    `Current Conversation ID: ${currentConversationId}`,
    `Conversation List: ${conversations}`,
  );
};
```
