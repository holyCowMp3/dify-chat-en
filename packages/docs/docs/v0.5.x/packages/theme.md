# `@dify-chat/theme`

![version](https://img.shields.io/npm/v/@dify-chat/theme) ![NPM Last Update](https://img.shields.io/npm/last-update/@dify-chat/theme) ![NPM Downloads](https://img.shields.io/npm/dm/@dify-chat/theme)

`@dify-chat/theme` is the theme package of the Dify Chat project, providing a complete theme management solution, including theme context hooks, theme switching components, theme state management, etc.

## Main Features

- Theme mode switching (system/light/dark)
- Complete theme context management
- Automatic dark mode adaptation

## Installation

Install via npm/yarn/pnpm:

```bash
# npm
npm install @dify-chat/theme

# yarn
yarn add @dify-chat/theme

# pnpm
pnpm add @dify-chat/theme
```

## API

### `<ThemeContextProvider />`

Theme context container.

> Note: Only when the upper component uses `ThemeContextProvider` to wrap the application can theme-related functionality be used in child components.

Use `ThemeContextProvider` in the outermost component to wrap the application:

```tsx
import { ThemeContextProvider } from '@dify-chat/theme';
function App() {
  return (
    <ThemeContextProvider>
      <YourApp />
    </ThemeContextProvider>
  );
}
```

### `<ThemeSelector />`

Theme selector component.

By default, `ThemeContextProvider` already provides the ability to adapt to system themes. If you need to support users manually switching theme modes, you can introduce the theme selector:

```tsx
import { ThemeSelector } from '@dify-chat/theme';

function App() {
  const { themeMode } = useThemeContext();
  return (
    <ThemeSelector>
      <Button>Current Theme Mode: {themeMode}</Button>
    </ThemeSelector>
  );
}
```

### `useThemeContext()`

Get theme context hook.

Return values:

- `theme`: Current application theme, value is `'light' | 'dark'`
- `themeMode`: Current theme mode, value is `'light' | 'dark' | 'system'`
- `setThemeMode`: Set theme mode, accepts a parameter of type `'light' | 'dark' | 'system'`

You can use the `useThemeContext` hook to get the current application theme:

```tsx
import { useThemeContext } from '@dify-chat/theme';

function ThemeToggle() {
  const { theme } = useThemeContext();

  return <div>Current Theme Mode: {theme}</div>;
}
```

You can also use the `setThemeMode` method in components to customize theme mode switching:

```tsx
import { useThemeContext } from '@dify-chat/theme';

function ThemeSwitcher() {
  const { themeMode, setThemeMode } = useThemeContext();
  return (
    <div>
      <h3>Current Theme Mode: {themeMode}</h3>

      <div>
        <Button onClick={() => setThemeMode('light')}>Light Mode</Button>
        <Button onClick={() => setThemeMode('dark')}>Dark Mode</Button>
        <Button onClick={() => setThemeMode('system')}>System Theme</Button>
      </div>
    </div>
  );
}
```

### Other Exported Members

**Enums**

- `ThemeEnum`: Theme enum
- `ThemeModeEnum`: Theme mode enum
- `ThemeModeLabelEnum`: Theme mode text enum

**Constants**

- `ThemeModeOptions` : Theme mode options

**Types**

- `IThemeContext`: Theme context type
- `IThemeMode`: Theme mode type
- `ICurrentTheme`: Current theme type

Usage example:

```tsx
import { Select } from 'antd';
import { ThemeModeEnum, ThemeModeOptions } from '@dify-chat/theme';

function ThemeSwitcher() {
  const { themeMode, setThemeMode } = useThemeContext();
  return (
    <div>
      <h3>Current Theme Mode: {themeMode}</h3>
      <div>
        Switch Theme Mode:
        <Select
          options={ThemeModeOptions}
          value={themeMode}
          onChange={(value) => setThemeMode(value as ThemeModeEnum)}
        />
      </div>
    </div>
  );
}
```
