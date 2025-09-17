# Organizing zIndices

_September 17th, 2025_

I've been working on a react native application that uses react-native-reanimated animations.

To prioritize the order of which components display over others during animations, I've been assigning semi-arbitrary `zIndex` values. Today I added a new animation and found myself jumping between files, writing down component names and their corresponding `zIndex`.

Here are my notes:

```
// InAppConsole -> 9999999999

// flashMessageGradientBehindUndoButton -> 9999
// flashMessage -> 999

// GroupSelectionNavigationBarOverlay -> 100
// NavigationBar -> 50
// NavigationBarDropdownMenu -> 10

// sendingComposedEmailLoadingSpinnerOverlay -> 20
// barAboveComposeKeyboard -> 10
```

Each of those `zIndex`'s are in distinct files.

Without the ability to see all those in one place and without the grouping-via-newline organization that I added, it'd be tough to tell which indices were importantly related to others.

- I want to see all those values in a single place, as above
- I also don't actually care about the zIndex values; just their order in the list

To solve that problem, I made a `zIndex.ts` file:

```ts
const zIndices = [
  "inAppConsole",

  "flashMessageButtonBackgroundGradient",
  "flashMessageBackground",

  "navigationBarGroupSelectOverlay",
  "navigationBarBackground",
  "navigationBarDropdownMenu",

  "sendComposedEmailLoadingSpinnerOverlay",

  "barAboveComposeKeyboard",
] as const

type zIndexNames = (typeof zIndices)[number]

export default (name: zIndexNames) =>
  [...zIndices, "everythingElseAtZindex0"].reverse().reduce(
    (acc, name, position) => {
      acc[name] = position
      return acc
    },
    {} as { [key: string]: number },
  )[name]
```

That function calculates this value:

```ts
{
    everythingElseAtZindex0: 0,
    barAboveComposeKeyboard: 1,
    sendComposedEmailLoadingSpinnerOverlay: 2,
    navigationBarDropdownMenu: 3,
    navigationBarBackground: 4,
    navigationBarGroupSelectOverlay: 5,
    flashMessageBackground: 6,
    flashMessageGradient: 7,
    InAppConsole: 8
  }
```

And then returns the value for the key passed to it.

It's used like this:

```ts
import { View } from "react-native"
import zIndex from "src/zIndex"

const NavigationBar = () =>
  <View style={{zIndex: zIndex("navigationBarBackground")}} />
```

Now I can decide the display priority for any new elements without the need to open several files or concern myself with arbitrary `zIndex` values.

Also, the `zIndex()` function call is type safe; so if I misspell the name of a zIndex, I'll get a typescript error.

```ts
zIndex("inAppConsole2")
```

> 1. Argument of type '"inAppConsole2"' is not assignable to parameter of type '"inAppConsole" | "flashMessageButtonBackgroundGradient" | "flashMessageBackground" | "navigationBarGroupSelectOverlay" | "navigationBarBackground" | "navigationBarDropdownMenu" | "sendComposedEmailLoadingSpinnerOverlay" | "barAboveComposeKeyboard"'. [2345]

I considered creating a more expressive data structure to more clearly communicate the relationship between importantly related things (I'm using newlines in the version above). That solution required some typescript back flips and recursive logic in the exported function that made my head hurt.

After staring at both versions, I think I prefer this one for now.

[All Posts](/README.md)
