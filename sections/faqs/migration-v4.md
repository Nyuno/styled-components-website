## What do I need to do to migrate to v4?

This is a pretty big release with lots of changes both under the hood and at the API level. As the beta progresses, we will try to release codemods to make the items below simpler. Also, if you find any issues with the steps below, please [leave constructive feedback](https://github.com/styled-components/styled-components-website/issues/296)!

We've also got an issue ready for beta feedback here: <https://github.com/styled-components/styled-components/issues/1969>

1. Upgrade to the styled-components beta:

```
npm install styled-components@beta
```

2. Make sure your application is using `react` >= 16.3; internally we are using the new `React.forwardRef` API and new context APIs if you wish to try and polyfill for older React version support

```
npm install react@^16.3 react-dom@^16.3
```

> If you are using `enzyme` or other dependencies like `react-test-renderer`, there may be more related upgrades to complete if you are coming from an old version of `react`.

3. If you are using the `.extend` API, switch your components to use `styled(StyledComponent)` instead.

🚫

```js
import styled from 'styled-components'

const Component = styled.div`
  background: blue;
  color: red;
`

const ExtendedComponent = Component.extend`
  color: green;
`
```

✅

```js
import styled from 'styled-components'

const Component = styled.div`
  background: blue;
  color: red;
`

const ExtendedComponent = styled(Component)`
  color: green;
`
```

See the ["extending styles" documentation](/docs/basics#extending-styles) for more examples.

4. If you were using the `injectGlobal` API to add global styles to your page, use the new [`createGlobalStyle` helper component](/docs/api#createglobalstyle) instead.

🚫

```jsx
import { injectGlobal } from 'styled-components'

injectGlobal`
  body {
    color: red;
  }
`
```

✅

```js
import { createGlobalStyle } from "styled-components"

const GlobalStyle = createGlobalStyle`
  body {
    color: red;
  }
`

// later in your app's render method
<React.Fragment>
  <Navigation />
  <OtherImportantTopLevelComponentStuff />
  <GlobalStyle />
</React.Fragment>
```

See the documentation for [`createGlobalStyle`](/docs/api#createglobalstyle) to see all the cool stuff you can do with it that wasn't possible before with `injectGlobal`!

5. If you were using the `innerRef` prop, change it to a normal `ref`.

🚫

```jsx
const Component = styled.div`
  background: blue;
  color: red;
`

// later in your render method
<Component innerRef={element => { this.myElement = element }}>Something something</Component>
```

✅

```js
const Component = styled.div`
  background: blue;
  color: red;
`

// later in your render method
<Component ref={element => { this.myElement = element }}>Something something</Component>
```

6. If you're using the `keyframes` component in a partial without the `css` helper, you'll need to use the helper now. In general, always use the css helper when composing styling partials to be interpolated into a styled component.

🚫

```jsx
import styled, { keyframes } from 'styled-components'

const animation = keyframes`
  0% {
    opacity: 0;
  }

  100 {
    opacity: 1;
  }
`

const animationRule = `
  ${animation} 1s infinite alternate
`

const Component = styled.div`
  animation: ${animationRule};
`
```

✅

```js
import styled, { css, keyframes } from 'styled-components'

const animation = keyframes`
  0% {
    opacity: 0;
  }

  100 {
    opacity: 1;
  }
`

const animationRule = css`
  ${animation} 1s infinite alternate;
`

const Component = styled.div`
  animation: ${animationRule};
`
```

7. If you're using TypeScript, the typings are now located in DefinitelyTyped:

```
npm install @types/styled-components
```

They have not been updated yet for v4, so help on that front is extremely welcome! We won't release a formal v4 version until they're in place.

That's it! Aside from migrating, we also highly recommend reading up on the new [`"as" prop`](/docs/api#as-polymorphic-prop) which is intended to replace the [`withComponent API`](/docs/api#withcomponent) in the future.
