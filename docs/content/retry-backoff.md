## Retry and backoff

1. [Maximum retries](#maximum-retries)
1. [Backoff algorithms](#backoff-algorithms)

### Maximum retries
You can set and get the maximum retries upon connection issues using the `OPT_MAX_RETRIES` option. Note that this is the number of _retries_, meaning if you set this option to _n_, there will be a maximum _n+1_ attempts overall. Defaults to 10.

##### *Example*

```php
$valkey->setOption(Redis::OPT_MAX_RETRIES, 5);
$valkey->getOption(Redis::OPT_MAX_RETRIES);
```

### Backoff algorithms
You can set the backoff algorithm using the `Redis::OPT_BACKOFF_ALGORITHM` option and choose among the following algorithms described in this blog post by Marc Brooker from AWS: [Exponential Backoff And Jitter](https://aws.amazon.com/blogs/architecture/exponential-backoff-and-jitter):

* Default: `Redis::BACKOFF_ALGORITHM_DEFAULT`
* Decorrelated jitter: `Redis::BACKOFF_ALGORITHM_DECORRELATED_JITTER`
* Full jitter: `Redis::BACKOFF_ALGORITHM_FULL_JITTER`
* Equal jitter: `Redis::BACKOFF_ALGORITHM_EQUAL_JITTER`
* Exponential: `Redis::BACKOFF_ALGORITHM_EXPONENTIAL`
* Uniform: `Redis::BACKOFF_ALGORITHM_UNIFORM`
* Constant: `Redis::BACKOFF_ALGORITHM_CONSTANT`

These algorithms depend on the _base_ and _cap_ parameters, both in milliseconds, which you can set using the `Redis::OPT_BACKOFF_BASE` and `Redis::OPT_BACKOFF_CAP` options, respectively.

##### *Example*

```php
$valkey->setOption(Redis::OPT_BACKOFF_ALGORITHM, Redis::BACKOFF_ALGORITHM_DECORRELATED_JITTER);
$valkey->setOption(Redis::OPT_BACKOFF_BASE, 500); // base for backoff computation: 500ms
$valkey->setOption(Redis::OPT_BACKOFF_CAP, 750); // backoff time capped at 750ms
```