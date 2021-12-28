# Tidy up your twitter timeline

Running this script will tidy up your twitter timeline by hiding retweets (but not quoted retweets), other peoples' likes, repeated tweets, and more.

By default, Twitter mutes keywords from people you don't follow. To stop this from happening, before running this script go to https://twitter.com/settings/add_muted_keyword, mute `RT @` and select "From Anyone", save it, and _then_ run the script.

```
location.assign("https://twitter.com/settings/muted_keywords");

function setNativeValue(element, value) {
  const valueSetter = Object.getOwnPropertyDescriptor(element, "value").set;
  const prototypeValueSetter = Object.getOwnPropertyDescriptor(
    Object.getPrototypeOf(element),
    "value"
  ).set;

  if (valueSetter && valueSetter !== prototypeValueSetter) {
    prototypeValueSetter.call(element, value);
  } else {
    valueSetter.call(element, value);
  }
}

function delay(f = 1) {
  return new Promise((res) => setTimeout(res, f * 1000));
}

`ActivityTweet
generic_activity_highlights
generic_activity_momentsbreaking
RankedOrganicTweet
suggest_activity
suggest_activity_feed
suggest_activity_highlights
suggest_activity_tweet
suggest_grouped_tweet_hashtag
suggest_pyle_tweet
suggest_ranked_organic_tweet
suggest_ranked_timeline_tweet
suggest_recap
suggest_recycled_tweet
suggest_recycled_tweet_inline
suggest_sc_tweet
suggest_timeline_tweet
suggest_who_to_follow
suggestactivitytweet
suggestpyletweet
suggestrecycledtweet_inline`
  .split(/\W+/)
  .reduce(async function go(prev, keyword) {
    await prev;

    document.querySelector('[aria-label="Add muted word or phrase"]').click();
    await delay();

    const el = document.querySelector("input[name=keyword]");
    setNativeValue(el, keyword);
    el.dispatchEvent(new Event("input", { bubbles: true }));
    await delay();

    document
      .evaluate(
        '//*[contains(text(), "Save")]/ancestor::*[@role = "button"]',
        document,
        null,
        XPathResult.ORDERED_NODE_SNAPSHOT_TYPE,
        null
      )
      .snapshotItem(0)
      .click();
    return delay();
  }, Promise.resolve());
```

Thank you to @dav1dnix and @jakebellacera for the script I'm expanding on a little here.
