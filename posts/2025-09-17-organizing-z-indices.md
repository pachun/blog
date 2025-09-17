# Organizing zIndices

_September 17th, 2025_

I've been working on a react native application that uses react-native-reanimated animations.

To prioritize the order of which components should be displayed over others, I'd been assigning things arbitrary `zIndex` values in their `style`s props. Once there were a handful of components with a `zIndex`, I found myself jumping between files, writing down component names and their corresponding `zIndex` in order to add new animations.

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

Each of those are in a different file. Without the organization I added above (grouping via newlines) it'd be hard to tell which indices were importantly related to others. As I thought about what a chore this was, I realized a few things:

- I want to see all those values in a single place, as above
- I also don't actually care about the numberical zIndex values; just their order in the list

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

And then returns the value for the key passed in. It's used like this:

```ts
import { View } from "react-native"
import zIndex from "src/zIndex"

const Thing = () => {
  <View style={{zIndex: zIndex("navigationBarBackground")}}>
  </View>
}
```

Now I can decide the priority for any new elements at a high level in a single place, without the need to open several files or concern myself with arbitrary numeric values.

Also, the `zIndex()` function call is type safe; so if I misspell the name of a zIndex, I'll get a typescript error.

```ts
zIndex("inAppConsole2")
```

> 1. Argument of type '"inAppConsole2"' is not assignable to parameter of type '"inAppConsole" | "flashMessageButtonBackgroundGradient" | "flashMessageBackground" | "navigationBarGroupSelectOverlay" | "navigationBarBackground" | "navigationBarDropdownMenu" | "sendComposedEmailLoadingSpinnerOverlay" | "barAboveComposeKeyboard"'. [2345]

I considered creating a more expressive data structure to more clearly communicate the relationship between importantly related things (I'm using newlines in the version above). That solution required some typescript back flips and recursive logic in the exported function that made my head hurt.

After staring at both versions, I think I prefer this one for now.

[All Posts](/README.md)
