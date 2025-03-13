# Use Expo Doctor

_March 13th, 2025_

You may have heard: React Native upgrades are hard.

If you use [Expo](1) (and you should because it's @#$%ing awesome) then there's something you should do to ensure your Expo SDK upgrades go [as smoothly as they should](3).

**use `npx expo-doctor`**

Use it often. Run it when you run `yarn tsc`. Run it when you run `yarn lint`. Run it when you run `yarn test`. Add it to your CI.

Expo is awesome at updating things, letting you know, and telling you how it affects your project specifically. A week never goes by without `yarn doctor` issuing new warnings (to put it lightly; updates are usualy sent at least once per day). If you're staying on top of those changes as they happen, you'll have a much better time upgrading Expo SDKs.

[1]: https://expo.dev
[3]: https://expo.dev/changelog/2024-11-12-sdk-52#%EF%B8%8F-upgrading-your-app

[All Posts](/README.md)
