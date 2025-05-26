# Use ESLint's `no-floating-promises` Rule in Your Test Environment

_May 26th, 2025_

I recently upgraded my personal project's [Expo][1] SDK to version 53 (from 52) and I started to see warnings in my test suite output:

> The current testing environment is not configured to support act(...)

A lot of them.
Enough to overflow my terminal's scroll buffer.
Every test still passed and I still had the right test coverage.

I ran each test file individually.
I have lots of these...
It took a little while but I found the offending file.

I ran each test in the file individually.
There were lots of those too...
Every test passed without any warnings.

I double checked that running the whole file showed the warnings. Yep.

I ran each test individually again, in case I accidentally skipped one earlier. Nope.

I started commenting tests out and running the whole file as I did.
That helped me find the offending test.
When it runs alone, it passes without warning.
When it's run with any other test, boom:

> The current testing environment is not configured to support act(...)

Lots of those.

```typescript
describe("when the inbox thread list is pulled down from the top", () => {
  it("refreshes inbox threads", async () => {
    const signinGmailAccount = gmailAccountFactory()

    signin({
      signinGmailAccount,
      inboxThreads: [gmailThreadFactory({ id: "thread-1" })],
    })

    await expectTestIdToBeVisible(
      "inbox-list-item-for-gmail-thread-with-id-thread-1",
    )

    mockGetThreadsRequest({
      request: {
        gmailAccountId: signinGmailAccount.id,
        params: { labelIds: "INBOX" },
      },
      response: { threads: [gmailThreadFactory({ id: "thread-2" })] },
      renderLoadingState: true,
    })

    await pullToRefreshTestId("inbox emails list")

    await expectTestIdToBeRefreshing("inbox emails list")
    await expectTestIdNotToBeRefreshing("inbox emails list")
    await expectTestIdNotToBeVisible(
      "inbox-list-item-for-gmail-thread-with-id-thread-1",
    )
    await expectTestIdToBeVisible(
      "inbox-list-item-for-gmail-thread-with-id-thread-2",
    )
  })
})
```

Given the title of this post, you can probably already see the problem.
I didn't and I'd already spent at least an hour figuring out which test was the problem.
Then I spent about 2 more hours figuring out that `signin()` call needed to be `await`ed.

This isn't the first time I've lost hours to that problem; Forgetting an `await` in a test.
I just turned on the [`no-floating-promises` Eslint rule][2] (only for my test suite) and I'm pretty pleased with myself becuase if I charged myself what I charge clients...
I just saved myself a lot of money.
Depending on how you look at it, I guess 🙃

[All Posts](/README.md)

[1]: https://expo.dev/changelog/sdk-53
[2]: https://typescript-eslint.io/rules/no-floating-promises/
