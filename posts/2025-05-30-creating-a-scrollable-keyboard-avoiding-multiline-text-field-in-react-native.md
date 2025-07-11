# Creating a Scrollable, Keyboard-Avoiding Multiline Text Field in React Native

_May 30th, 2025_

Today I spent a few hours trying tocreate a keyboard-avoiding multiline text field in React Native and discovered that it's pretty tough. I did manage to arrive at [a solution that I'm reasonably happy with][1] (<- That's a 15 second video that GH will make you download).

[Here's a demo repository with all of the code, context and history][2].

Here's the TL;DR of it:

```tsx
import React from "react"
import { ScrollView, Keyboard, TextInput, View, Button } from "react-native"
import Animated, {
  useAnimatedKeyboard,
  useAnimatedStyle,
} from "react-native-reanimated"
import {
  SafeAreaProvider,
  useSafeAreaInsets,
} from "react-native-safe-area-context"

const App = () => {
  return (
    <SafeAreaProvider>
      <KeyboardAvoidingMultilineTextInput />
    </SafeAreaProvider>
  )
}

const KeyboardAvoidingMultilineTextInput = () => {
  const fieldRef = React.useRef<TextInput>(null)
  const [fieldValue, setFieldValue] = React.useState("")
  const [isScrollingUnfocusedField, setIsScrollingUnfocusedField] =
    React.useState(false)

  const { bottom: homeBarHeight, top: statusBarHeight } = useSafeAreaInsets()
  const keyboard = useAnimatedKeyboard()
  const keyboardSpaceTakerStyle = useAnimatedStyle(() => ({
    height:
      keyboard.height.value > homeBarHeight
        ? keyboard.height.value
        : homeBarHeight,
    width: "100%",
  }))

  return (
    <View
      style={{
        flex: 1,
        paddingTop: statusBarHeight,
      }}
    >
      <View style={{ alignItems: "center" }}>
        <Button title="Drop Keyboard" onPress={() => Keyboard.dismiss()} />
      </View>
      <ScrollView
        style={{ flex: 1, backgroundColor: "#c2c2c2" }}
        contentContainerStyle={{ flexGrow: 1 }}
        onScrollBeginDrag={() => {
          if (!fieldRef.current?.isFocused()) {
            setIsScrollingUnfocusedField(true)
          }
        }}
        onScrollEndDrag={() => setIsScrollingUnfocusedField(false)}
      >
        <TextInput
          ref={fieldRef}
          value={fieldValue}
          onChangeText={setFieldValue}
          style={{
            flex: 1,
            padding: 16,
            fontSize: 16,
          }}
          multiline
          textAlignVertical="top"
          scrollEnabled={false}
          editable={!isScrollingUnfocusedField}
        />
      </ScrollView>
      <Animated.View style={keyboardSpaceTakerStyle} />
    </View>
  )
}

export default App
```

The tricky part was managing when the `ScrollView` vs the `TextInput` was in charge of scrolling. These props on the `ScrollView` and `TextInput` were the magic bits for me - that even ChatGPT, curating the world's wealth of knowledge, wasn't able to help me with. So today I got to feel like a pioneer.

```tsx
onScrollBeginDrag={() => {
  if (!fieldRef.current?.isFocused()) {
    setIsScrollingUnfocusedField(true)
  }
}}
onScrollEndDrag={() => setIsScrollingUnfocusedField(false)}

// ...

editable={!isScrollingUnfocusedField}
```

There are a few drawbacks to my approach; it requires react native reanimated. I didn't mind for my purposes, because I was already using it. There's also a bit of a delay when you tap to edit text low in the field - the keyboard rises and covers the text before the scrollview animates to compensate. It's not too bad, but it's definitely noticeable.

If you're curious (in truth, I just want you to look), here's a snapshot of the work in progress screen I'm using it for; It's an email client.

<p align="center">
  <img src="/posts/assets/2025-05-30-creating-a-scrollable-keyboard-avoiding-multiline-text-field-in-react-native/emma.png" height="500" alt="emma" />
</p>

Hope it helps someone... 🙏

[All Posts](/README.md)

[1]: /posts/assets/2025-05-30-creating-a-scrollable-keyboard-avoiding-multiline-text-field-in-react-native/demo-video.mp4
[2]: https://github.com/pachun/scrollable-multiline-RN-text-input
