# Use Expo Doctor Often

_March 13th, 2025_

You may have heard: React Native upgrades are hard.

If you use [Expo](1) (and you should because it's @#$%ing awesome) then there's something you should do to ensure your Expo SDK upgrades go [as smoothly as they should](3).

**use `npx expo-doctor`**

([expo-doctor docs](4))

Add it to your CI. Use it as often as you run `yarn tsc`, `yarn lint` and `yarn test`.

Expo is awesome at updating things, letting you know, and telling you how it affects your project specifically. A week never goes by without `npx expo-doctor` issuing new warnings. If you're staying on top of those changes as they happen, you'll have a much better time upgrading Expo SDKs.

[1]: https://expo.dev
[3]: https://expo.dev/changelog/2024-11-12-sdk-52#%EF%B8%8F-upgrading-your-app
[4]: https://docs.expo.dev/develop/tools/#expo-doctor

[All Posts](/README.md)
