# SolidJS Auth0 Wrapper

A SolidJS implementation for Auth0 using the package `@auth0/auth0-spa-js`. This work is based on top of the library created by [@rturnq](https://github.com/rturnq/solid-auth0) (which hasn't been updated in a few years).

# Getting Started

## Installation

### NPM

```
npm i @afroze9/solid-auth0
```

### PNPM

```
pnpm add @afroze9/solid-auth0
```

### Yarn

```
npm add @afroze9/solid-auth0
```

## Usage

### At the root of your app

Note: Please ensure you have an SPA setup in Auth0 to configure the context.

```tsx
import { Auth0 } from '@afroze9/solid-auth0';

const App: Component = () => {
  return (
    <Auth0
      domain="_auth0_domain_"
      clientId="_client_id_"
      audience="_optional_api_audience_"
      logoutRedirectUri={`${window.location.origin}/`}
      loginRedirectUri={`${window.location.origin}/`}
      scope="_optional_scopes_"
    >
      <AppComponent />
    </Auth0>
  );
};
```

### Elsewhere in the app

```tsx
import { Auth0State, useAuth0 } from '@afroze9/solid-auth0';

const AppComponent: Component = () => {
  const auth0 = useAuth0();

  return (
    <>
      <div>
        <Switch
          fallback={
            <button id="login" onClick={() => auth0?.loginWithRedirect()}>
              Login
            </button>
          }
        >
          <Match when={auth0 === undefined || auth0.isLoading()}>
            <p>Loading...</p>
          </Match>
          <Match when={auth0?.user?.()}>
            {(user) => (
              <>
                <img src={user().picture} alt="user profile picture" />
                <p>
                  Logged in as {user().name ?? ''} ({user().email ?? ''})
                </p>
                <button id="logout" onClick={() => auth0!.logout()}>
                  Logout
                </button>
              </>
            )}
          </Match>
        </Switch>
      </div>
    </>
  );
};
```

### To secure a route

```tsx
import { Protected } from '@afroze9/solid-auth0';

const MyPage: Component = () => {
  return <div>This is a secure route</div>;
};

// You can pass any component to onRedirecting
export default () => {
  <Protected onRedirecting={<div>Redirecting...</div>}>
    <MyPage />
  </Protected>;
};
```

# Notes

Currently the wrapper only supports a few underlying functions/properties from the original `@auth0/auth0-spa-js` package. Future plan is to add more functionality.
