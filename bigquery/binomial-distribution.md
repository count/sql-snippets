# Generating a binomial distribution

Explore this snippet [here](https://count.co/n/F6GnYbjyIct?vm=e)

# Description

Once it is possible to generate a uniform distribution of numbers, it is possible to generate any other distribution. This snippet shows how to generate a sample of numbers that are binomially distributed using rejection sampling.

> Note - rejection sampling is simple to implement, but potentially very inefficient

The steps here are:
- Generate a random list of `k` = 'number of successes' from n trials, with each trial having an expected `p` = 'probability of success'.
- For each `k`, calculate the binomial probability of `k` successes using the [binomial formula](https://en.wikipedia.org/wiki/Binomial_distribution), and another random number `u` between zero and one.
- If `u` is smaller than the expected probability, accept the value of `k`, otherwise reject it

```sql
with params as (
  select
    0.3 as p,             -- Probability of success per trial
    20 as n,              -- Number of trials
    100000 as num_samples -- Number of samples to test (the actual samples will be fewer)
),
samples as (
  select
    -- Generate a random integer k = 'number of successes' in [0, n]
    mod(abs(farm_fingerprint(cast(indices as string))), n + 1) as k,
    -- Generate a random number u uniformly distributed in [0, 1]
    mod(abs(farm_fingerprint(cast(indices as string))),  1000000) / 1000000 as u
  from
    params,
    unnest(generate_array(1, num_samples)) as indices
),
probs as (
  select
    -- Hack in a factorial function for n! / (k! * (n - k)!)
    (select exp(sum(ln(i))) from unnest(if(n = 0, [1], generate_array(1, n))) as i) /
    (select exp(sum(ln(i))) from unnest(if(k = 0, [1], generate_array(1, k))) as i) /
    (select exp(sum(ln(i))) from unnest(if(n = k, [1], generate_array(1, n - k))) as i) *
    pow(p, k) * pow(1 - p, n - k) as threshold,
    k,
    u
  from params, samples
),
after_rejection as (
  -- If the random number for this value of k is too high, reject it
  select k from probs where u <= threshold
)

select * from after_rejection
```

| k   |
| --- |
| 7   |
| 1   |
| 6   |
| ... |