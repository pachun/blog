# I Made a CI Dashboard for My Living Room

_April 16th, 2025_

When I was in high school, I kept my report card under the peice of glass on my desk that protected the surface from all the coke can rings. I kept my most recent tests pinned to the wall above the desk. It helped me stay focused and kept my goals top of mind.

When I worked at Pivotal Labs, teams of 4-15 people worked at a single big desk, at the end of which was a monitor showing the statuses of our project's CI/CD servers. It helped us stay focused and kept the project top of mind. It also really helps you notice when a merge breaks the app or a deploy fails. It's a lot less easy to trigger a merge, close a browser tab and then realize hours later that you broke your build ten minutes after triggering it. I could extol the virtues of having a CI/CD monitor visible for a while...

The point is, I made one for my personal project at home.

<p align="center">
  <img src="/posts/assets/2025-04-16-i-made-a-ci-dashboard-for-my-living-room/photo.png" width="600" alt="photo" />
</p>

I was cleaning out that one closet in my house that had a bunch of dragons (unorganized piles of old clothes, boxes of tech, old furniture, etc) in it. The kind of place you keep out of sight so that it stays out of mind. Yesterday, while I was cleaning it, I found a coffee table, monitor, and my old laptop and thought...

💡 I could clean the closet _and_ keep my personal project's CI status top of mind.

Physically cleaning the closet and setting up the CI dashboard in my living room took a few hours. Getting the dashboard to actually work took slightly longer, I think. I spent about three hours today getting something running that I'm happy with.

The first thing I tried was leaving two Safari windows on, sharing the screen real estate with two vertical columns. I left it at [my GitHub actions "All Workflows" screen and filtered it to only show `main` branch workflows](https://github.com/pachun/react-native-use-app-lifecycle/actions?query=branch%3Amain). I did that for the api of the project, and the react native mobile app's repositories. Then I triggered a build and waited... Nothing happened. The page needed to refresh, but it had no way of knowing.

I thought about downloading chrome and an extension to auto reload the page every minute or so. I don't love polling, but it'd work for the quick setup I wanted. Then I realized that the screen would be flashing every minute when the page reloaded... I can live with polling but I _really_ don't like a flashing screen in my living room.

I tried to find something - an app, plugin, whatever - that already existed to show me those statuses on a dedicated monitor, but I was surprised that I couldn't find anything - and I wasn't being _super_ picky.

So, instead, I (shut up, ChatJippity) made a rails app that has an endpoint that I plugged into my github project's settings dashboard. When I trigger new builds, GitHub lets me know and hits the webhook endpoint. Then ActionCable lets the view know that the status of the build changed. I'm pretty happy with the result. It works exactly how I wanted and [you can use it too if that turns your crank](https://github.com/pachun/gh_ci_monitor).

<p align="center">
  <img src="/posts/assets/2025-04-16-i-made-a-ci-dashboard-for-my-living-room/screenshot.png" width="600" alt="screenshot" />
</p>

[All Posts](/README.md)
