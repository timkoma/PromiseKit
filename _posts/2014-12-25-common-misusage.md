---
category: home
layout: default
---

# Common Misusage

Here a few common misusages of promises that are worth avoiding.

## Superfluous use of `+new:`

When you already have a promise, you don’t need `+new:`, `+new:` is only
required when you are wrapping something that is *not* a promise. So
don’t do this:

{% highlight objectivec %}
- (PMKPromise *)fetchParseAndStore {
    return [PMKPromise new:^(PMKFulfiller fulfill, PMKRejecter reject){
        [Thing fetch].then(^(id result){
            return [Thing parse:result];
        }).then(^(id result){
            return [Thang store:result];
        }).then(^(id result){
            fulfill(result);
        }).catch(^(NSError *error){
            reject(error);
        });
    }];
}
{% endhighlight %}

Instead do this:

{% highlight objectivec %}
- (PMKPromise *)fetchParseAndStore {
    return [Thing fetch].then(^(id result){
        return [Thing parse:result];
    }).then(^(id result){
        return [Thang store:result];
    }).then(^(id result){
        fulfill(result);
    });
}
{% endhighlight %}

You already *had* a promise, so there’s no need to wrap it in *another*
promise.

Although it is worth noting, that we could have just passed `fulfill` and
`reject` straight to `then` and `catch`. This is a handy trick on
ocassion negating the need for a block to wrap a single call to a block:

 {% highlight objectivec %}
 - (PMKPromise *)fetchParseAndStore {
     return [PMKPromise new:^(PMKFulfiller fulfill, PMKRejecter reject){
         [Thing fetch].then(^(id result){
             return [Thing parse:result];
         }).then(^(id result){
             return [Thang store:result];
         }).then(fulfill).catch(reject);
     }];
 }
 {% endhighlight %}
 
However we are demonstrating this for instructional purposes. With the
above situation, don’t use `+new:`.

<div><a class="pagination" href="/appendix">Next: Appendix</a></div>
