<p align="center">
  <img alt="react-theme-provider" src="./assets/theme-provider-logo.png" width="496">
</p>

---
## About 
`react-theme-provider` is a set of utilities help you create theming system in few easy steps.
You can use it to provide a custom theme to customize the colors, fonts etc. 

## Features
 - works in react and react-native
 - `ThemeProvider` component
 - `withTheme` HOC
 - `createTheming(defaultTheme)` - factory returns `ThemeProvider` component and `withTheme` HOC with default theme injected.

## Getting started
### Instalation
```
npm install --save @callstack/react-theme-provider
```
or using yarn
```
yarn add @callstack/react-theme-provider
```

### Usage
To use, simply wrap your code into `ThemeProvider` component and pass your theme as a `theme` prop.

```js
<ThemeProvider theme={{ primaryColor: 'red', background: 'gray'}}>
  <App />
</ThemeProvider>
```

You could access theme data inside every component by wraping it into `withTheme` HOC. Just like this:

```js
class App extends React.Component {
  render() {
    return (
      <div style={{ color: props.theme.primaryColor }}>
        Hello
      </div>
    );
  }
}

export default withTheme(App);
```

## `ThemeProvider`
**type:** `React.ComponentType<{ children?: any, theme?: T }>`

Component you have to use to provide theme to any component wrapped in `withTheme` HOC.

### props
 -`theme` - your theme object

## `withTheme`
**type:** `(React.ComponentType<*>) => React.ComponentType<*>`

Classic Higher Order Component which takes your component as an argument and inject `theme` prop with your theme into it.

### Example of usage
```js
const App = ({ theme }) => (
  <div style={{ color: theme.primaryColor }}>
    Hello
  </div>
);

export withTheme(App);
```

### Injected props
It will inject below props to component:
 - `theme` - our theme object.
 - `getWrappedInstance` -  exposed by some HOCs like react-redux's connect.
 Use it to get the ref to the underlying element.
 - `setNativeProps` - used by `Animated` (in React Native) to set props on the native component.

### Injecting theme by a direct prop
You can also override `theme` provided by `ThemeProvider` by setting `theme` prop on component wrapped in `withTheme` HOC.

Just like this:
```js
const Button = withTheme(({ theme }) => (
  <div style={{ color: theme.primaryColor }}>
    Click me
  </div>
));

const App = () => (
  <ThemeProvider theme={{ primaryColor: 'red' }}>
    <Button theme= {{ primaryColor: 'green' }}/>
  </ThemeProvider>
)
```
In this example Button will have green text.

## `createTheming`
**type:** `<T>(defaultTheme: T) => ThemingType<T>`

This is more advenced replacement to classic importing `ThemeProvider` and `withTheme` directly from library.

Returns instance of `ThemeProvider` component and `withTheme` HOC. 
You can use this factory to create singletone with your instances of `ThemeProvider` and `withTheme`.

### Example of usage
```js
// theming.js
import { createTheming } from 'react-theme-provider';
const { ThemeProvider, withTheme } = createTheming({
  primaryColor: 'red',
  secondaryColor: 'green',
});
export { ThemeProvider, withTheme };

//App.js
import { ThemeProvider, withTheme } from './theming';
```

### Arguments
 - `defaultTheme` - default theme object

### Benefits
 - Possibility to define flow type for your theme
 - Possibility to pass default theme
 - Unique context key for each instance so you can use multiple `ThemeProvider` in your app without any conflicts.

## Using with `flow`
In order to properly type your theme you have to create `ThemeProvider` using `createTheming` and type values it returns.

### example
```js
// theming.js
import { createTheming } from 'react-theme-provider';

import type { ThemingType } from 'react-theme-provider';

export type Theme = {
  primaryColor: string,
  accentColor: string,
  backgroundColor: string,
};

const { ThemeProvider, withTheme }: ThemingType<Theme> = createTheming(
  themes.default
);

export { ThemeProvider, withTheme };
```