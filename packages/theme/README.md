# `@dify-chat/theme`

![version](https://img.shields.io/npm/v/@dify-chat/theme) ![NPM Last Update](https://img.shields.io/npm/last-update/@dify-chat/theme) ![NPM Downloads](https://img.shields.io/npm/dm/@dify-chat/theme)

`@dify-chat/theme` is the theming package for the Dify Chat project. It delivers a complete theme management solution, including context hooks, theme switchers, and state management utilities.

## Key Features

- Theme mode switching (system / light / dark)
- Comprehensive theme context management
- Automatic dark-mode adaptation

## Installation

Install via npm, yarn, or pnpm:

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

Theme context provider.

> Note: Child components can only access theme features if the app is wrapped with `ThemeContextProvider`.

Wrap your root component with `ThemeContextProvider`:

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

By default, `ThemeContextProvider` adapts to the system theme. If you want users to choose a theme themselves, include the selector:

```tsx
import { ThemeSelector } from '@dify-chat/theme';

function App() {
  const { themeMode } = useThemeContext();
  return (
    <ThemeSelector>
      <Button>Current theme mode: {themeMode}</Button>
    </ThemeSelector>
  );
}
```

### `useThemeContext()`

Hook to read and control the theme context.

Returns:

- `theme`: current theme, `'light' | 'dark'`
- `themeMode`: active theme mode, `'light' | 'dark' | 'system'`
- `setThemeMode`: function to update the theme mode, accepts `'light' | 'dark' | 'system'`

Example of reading the current theme:

```tsx
import { useThemeContext } from '@dify-chat/theme';

function ThemeToggle() {
  const { theme } = useThemeContext();

  return <div>Current theme: {theme}</div>;
}
```

You can also use `setThemeMode` to customize the theme switcher:

```tsx
import { useThemeContext } from '@dify-chat/theme';

function ThemeSwitcher() {
  const { themeMode, setThemeMode } = useThemeContext();
  return (
    <div>
      <h3>Current theme mode: {themeMode}</h3>

      <div>
        <Button onClick={() => setThemeMode('light')}>Light mode</Button>
        <Button onClick={() => setThemeMode('dark')}>Dark mode</Button>
        <Button onClick={() => setThemeMode('system')}>System theme</Button>
      </div>
    </div>
  );
}
```

### Additional Exports

**Enums**

- `ThemeEnum`: Theme values
- `ThemeModeEnum`: Theme mode values
- `ThemeModeLabelEnum`: Text labels for theme modes

**Constants**

- `ThemeModeOptions`: Theme mode options

**Types**

- `IThemeContext`: Theme context shape
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
      <h3>Current theme mode: {themeMode}</h3>
      <div>
        Switch theme mode:
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
