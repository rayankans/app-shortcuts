# Shortcuts v2 Explainer

## Authors:
- [Rayan Kanso](https://github.com/rayankans) (Google)
- [Aaron Gustafson](https://github.com/aarongustafson) (Microsoft)

## Participate

- [Issue Tracker](https://github.com/rayankans/shortcuts/issues)

# Introduction

Web apps recently introduced the capability of defining static app shortcuts
- [v1 Explainer](https://github.com/MicrosoftEdge/MSEdgeExplainers/blob/master/Shortcuts/explainer.md)
- [Spec](https://w3c.github.io/manifest/#shortcuts-member)

This explainer aims to provide a way of dynamically creating app shortcuts, on top of the static shortcuts. This is a capability native apps already take advantage of. Dynamic shortcuts can provide a personalized experience to users which can facilitate re-engagement with key tasks.

# Goals
- Enable developers to provide dynamic shortcuts to a web app.

# Non-goals
- Provide a mechanism to add/update shortcuts in the background.

# Use cases

Use cases for this API are [defined in a separate document](./use-cases.md).

# API
The API will re-use the [`ShortcutItem`](https://w3c.github.io/manifest/#dom-shortcutitem) dictionary defined in v1.

## Setting shorcuts

```js
// Shortcut display order is based on the order they are provided in.
await navigator.shortcuts.set([{
  name: 'shortcut 1',
  short_name: 's1',
  description: 'A description of this shortcut',
  url: '/s1?shortcut',
  icons: [{src: '/icons/s1.png', sizes: '128x128'}],
}, {
  ...
}]);
```

## Adding a shortcut

```js
// Returns everything previously provided by calling navigator.shortcuts.set()
const shortcuts = await navigator.shortcuts.getAll();

await navigator.shortcuts.set([...shortcuts, {
  name: 'new shortcut',
  url: '/new?shortcut',
}]);
```

## Removing a shortcut

```js
const shortcuts = await navigator.shortcuts.getAll();
await navigator.shortcuts.set(shortcuts.filter(s => filterFn(s)));
```

## Changing shortcut order

```js
const shortcuts = await navigator.shortcuts.getAll();

if (shortcuts.length > 1) {
  // Swap first 2 elements
  [shortcuts[0], shortcuts[1]] = [shortcuts[1], shortcuts[0]];
  await navigator.shortcuts.set(shortcuts);
}
```

## Modifying a shortcut

```js
const shortcuts = await navigator.shortcuts.getAll();
shortcuts[0].name += '!';
shortcuts[0].icons[0].src = '/icons/new_shortcut_icon.png';
await navigator.shortcuts.set(shortcuts);
```

# Key scenarios
The main scenario the API will enable is providing personalized shortcuts to users.

A few examples of that would be:
- Chat apps: starting a conversation with most talked-to friends.
- Navigation apps: quick access to frequently visited locations.
- Productivity apps: opening recently added files.
- Music/Video apps: quick access to recently downloaded content.

# Detailed design discussion

## When should the API be enabled?
Interacting with dynamic shortcuts only makes sense on pages that have a web app manifest defined. Using the API on documents without web app manifests should throw an error. This is necessary since the dynamic shortcuts will have to be tied to an instance of a web app manifest.

## Should static shortcuts be exposed when using API?
No, this will add little to no value since:
- Static shortcut definitions are already available in the web app manifest.
- Static shortcuts being immutable will likely lead to complexity in using the API (or cause the API to become more complex to account for that case).

## Should an option to disable shortcuts be provided?
This is an Android feature which allows developers to disable shortcuts to handle pinned shortcuts. Given that this would not make sense within the context of other platforms, and there are available workarounds on Android, providing this capability is not necessary.

## Should shortcuts be exposed in the background (i.e in ServiceWorkers)?
Shortcuts are tied to a web app manfiest. Exposing them in a service worker would not make sense, since it would be ambiguous which list of shortcuts will need to be returned.

# Stakeholder Feedback / Opposition
TODO

# References & acknowledgements
Many thanks for valuable feedback and advice from:

- [Kenneth Rohde Christiansen](https://gist.github.com/kenchris) (Intel)
- [Matt Giuca](https://github.com/mgiuca) (Google)

Who worked on the v1 proposal and included forward-looking considerations for the v2 proposal.
