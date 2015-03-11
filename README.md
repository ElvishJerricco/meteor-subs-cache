# Meteor Subscription Cache (STILL A WORK IN PROGRESS)

This package is similar to [`meteorhacks:subs-manager`](https://github.com/meteorhacks/subs-manager). The goal is to cache subscriptions. Suppose you are subscribing to a post using `sub = Meteor.subscribe('post', postId)`. When the user clicks on another page, you typically want to stop the subscription (unsubscribe) to clean up using `subs.stop()`. But what if the user clicks back immediately after clicking something else? This is why you use `SubsCache.subscribe` to cache subscriptions.


## `meteorhacks:subs-manager` comparison

1. SubsManager [will stop your subscription "expireIn" minutes after your subscribe](https://github.com/meteorhacks/subs-manager/blob/master/lib/sub_manager.js#L94). SubsCache will stop your subscription "expireAfter" minutes after you stop (or the current reative computation stops).


2. SubsManager does not have a ready function for each subscription. [subsManager.ready](https://github.com/meteorhacks/subs-manager/blob/master/lib/sub_manager.js#L110) tells you if all cached subscriptions are ready. SubsCache as `subsCache.allReady()` and individual `sub.ready()`

3. SubsManager does not allow you to have subscriptions with different expiration times in the same cache. SubsCache allows you set the default expireAfter upon initialization but you can use `subscribeFor(expireAfter, ...)` to subscribe and cache for a different time.

4. SubsManager does not allow infinite items in a cache. SubsCache does if you set `cacheLimit` to -1.


5. SubsManager does not allow subscriptions that never expire. SubsCache does if you set `expireAfter` to -1.

## Getting Started

    meteor add ccorcos:subs-cache

Initialize with optional `expireAfter` (default 5) and `cacheLimit` (default 10). `expireAfter` is the number of minutes after a subscription is stopped without having been restarted before truely stopping it. If set to -1, the subscription will never expire. `cacheLimit` is the max number of subscriptions to cache. Set to -1 for unlimited capacity. 

    subsCache = new SubsCache({
      expireAter: 5,  // minutes
      cacheLimit: 10
    });

- `subsCache.allReady()` tells you if all subscriptions in the cache are ready

- `subsCache.subscribe(...)` creates a subscription just like `Meteor.subscribe`

- `subsCache.subscribeFor(expireIn, ...)` allow you to set the expiration other than the defualt.

- `subsCache.clear()` will stop all subscription immediately