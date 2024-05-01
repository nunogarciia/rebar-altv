# API

API allows for plugins to register their own APIs and make them available globally.

However, they are limited to either server-side or client-side.

This reduces the complexity to import functions from other plugins, or export functions for other plugins.

## How to Declare an API

1. Ensure you have an API setup in your plugin.

```ts
export function useMyCoolAPI() {
    function logPlayerName(player: alt.Player) {
        console.log(player.name);
    }

    return {
        logPlayerName,
    };
}
```

2. Create a global declaration for your API.

```ts
import { useApi } from '@Client/api/index.js';

export function useMyCoolAPI() {
    function logPlayerName(player: alt.Player) {
        console.log(player.name);
    }

    return {
        logPlayerName,
    };
}

// Declare global to TypeScript recognizes the typings
declare global {
    export interface ClientPlugin {
        ['my-cool-api']: ReturnType<typeof useMyCoolAPI>;
    }
}

// Really important to execute the return of your function
useApi().register('my-cool-api', useMyCoolAPI());
```

3. Done

## How to Get an API

This is all that's necessary to start working with other plugin APIs

```ts
import { useApi } from '@Client/api/index.js';

const myCoolAPI = useApi().get('my-cool-api');

function someFunction(somePlayer: alt.Player) {
    myCoolAPI.logPlayerName(somePlayer);
}
```