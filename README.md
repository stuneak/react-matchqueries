# react-matchqueries

[![npm version](https://badge.fury.io/js/react-matchqueries.svg)](https://badge.fury.io/js/react-matchqueries) ![](https://img.shields.io/npm/dm/react-matchqueries.svg)

> Library to control the design of your application using media queries 🔥

## Install

Via [NPM](https://docs.npmjs.com/):
```bash
npm install react-matchqueries --save
```

Via [Yarn](https://yarnpkg.com/en/):
```bash
yarn add react-matchqueries
```

## Usage

### `MatchQueriesProvider`

```javascript
import { MatchQueriesProvider } from 'react-matchqueries';

const queries = {
    isMobile: 'screen and (max-width: 767px)',
    isTablet: 'screen and (min-width: 768px) and (max-width: 1023px)',
    isDekstopS: 'screen and (min-width: 1024px) and (max-width: 1365px)',
    isDekstop: 'screen and (min-width: 1366px) and (max-width: 1440px)',
    isDekstopL: 'screen and (min-width: 1441px)'
}

export const Route = (props) =>  (
  <MatchQueriesProvider queries={queries}>
    <MyApp />
  </MatchQueriesProvider>
);

```

### `MatchQueriesConsumer`

```javascript
import { MatchQueriesConsumer } from 'react-matchqueries';

export const MyApp = () => (
  <MatchQueriesConsumer>
    {({ isDekstopS, isDekstop, isDekstopL, isTablet, isMobile }) => (
      <>
        {(isDekstopS || isDekstop || isDekstopL) && <p>Desktop View</p>}
        {(isTablet || isMobile) && <p>Tablet and Mobile View</p>}
      </>
    )}
  </MatchQueriesConsumer>
);


```

### Server Side Rendering

You can pass in media features from your server, all supported values can be found [here](https://www.w3.org/TR/css3-mediaqueries/#media1).

**Usage (matches mobile screen during SSR):**
```javascript
import { MatchQueriesProvider } from 'react-matchqueries';

const Route = () => {
  const values = {
    width: 300,
    type: 'screen',
  };

  return (
    <MatchQueriesProvider values={values}>
      <MyApp />
    </MatchQueriesProvider>
  );
};
```

#### React 16 ReactDOM.hydrate

It's very important to realise a server client mismatch is dangerous when using hydrate in React 16, ReactDOM.hydrate
can cause very [strange](https://github.com/facebook/react/issues/10591) html on the client if there is a mismatch.
To mitigate this we use the two-pass rendering technique mentioned in the React [docs](https://reactjs.org/docs/react-dom.html#hydrate).
We render on the client in the first pass using `values` with `css-mediaquery` used on the server, then we use the `matchmedia`
to get it's actual dimensions and render again if it causes different query results. This means there should be no React
server/client mismatch warning in your console and you can safely use hydrate. As a result of above, if you are server side rendering and using `ReactDOM.hydrate` you must supply `MatchQueriesConsumer` a `values` prop.
