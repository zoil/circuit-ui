# Fonts

## Defining the font stack

The font stack can be configured as part of the theme. The default font stack is applied to the `body` and the mono font stack is applied to `pre` and `code` tags. The default values are:

```js
export const fontStack = {
  default: 'aktiv-grotesk, -apple-system, BlinkMacSystemFont, "Segoe UI"',
  mono: 'Consolas, monaco, monospace'
};
```

## Loading custom web fonts

There are myriads of ways to load fonts on the web. You can choose between [FOIT](https://css-tricks.com/fout-foit-foft/), [FOUT](https://www.zachleat.com/web/comprehensive-webfonts/#fout-class) and [FOFT](https://www.zachleat.com/web/comprehensive-webfonts/#foft), each with several implementation techniques.

Circuit UI does not enforce any approach. It is up to you to choose and implement your preferred one. Have a look at Zach Leatherman's [Comprehensive Guide to Font Loading Strategies](https://www.zachleat.com/web/comprehensive-webfonts/) for inspiration.

The simplest strategy is the [unceremonious font-face](https://www.zachleat.com/web/comprehensive-webfonts/#font-face) with `preload` and `font-display`. This is the default approach recommended by [Google Fonts](https://fonts.google.com/). Here's how you can implement it with Circuit UI:

1. Add the font-face(s) that you'd like to use to you global HTML `<head>`:

```html
<link rel="preload" href="font-lato/lato-regular-webfont.woff2" as="font" type="font/woff2" crossorigin>
<style>
  @font-face {
    font-family: Lato;
    src: url('font-lato/lato-regular-webfont.woff2') format('woff2'), url('font-lato/lato-regular-webfont.woff') format('woff');
    font-display: swap; /* optional */
  }
</style>
```

2. Update the font stack in your theme:

```js
export const fontStack = {
  regular: '"Lato", sans-serif';
};
```

At SumUp, we like to use [FOFT, or FOUT with Two Stage Render](https://www.zachleat.com/web/comprehensive-webfonts/#foft) paired with [Critical FOFT with preload](https://www.zachleat.com/web/comprehensive-webfonts/#critical-foft-preload). When you opt for this approach, you will have to add some custom global styles. Here's how you can do this with Circuit UI:

When you call `globalStyles()` to inject the global styles, you can pass it your custom styles:

```js
import { yourTheme } from './styles/themes';
import { globalStyles } from 'circuit-ui/styles';

const customStyles = `
  body {
    font-family: sans-serif;
  }
  body.fonts-loaded-initial {
    font-family: 'LatoInitial', sans-serif;
  }
  body.fonts-loaded-full {
    font-family: 'Lato', sans-serif;
  }
`;

globalStyles({ theme: yourTheme, custom });
```
