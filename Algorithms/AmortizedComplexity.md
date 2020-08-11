> When we're appending to our sequence, we do not care so much about how long the individual 
> append operations take.\
> Rather, we are interested in how long an append operation takes on average.\
> Consider building up a sequence of a million elements using the append operation.\
> It really doesn't matter to us if 10 or 20 of those million operations take a very long time.\
> What matters is the amount of time it takes to do all the appends.\
> This is still a function of n, the ultimate size of the sequence, but it is different than the t(n) function that we have looked at so far, which measures the time taken by a single append operation.\
> Let's call this new function amortized time, or a(n), and define it as the average of all t(i) for 0 ≤ i ≤ n:\

https://scabl.blogspot.com/2014/10/what-heck-is-amortized-constant-time.html
