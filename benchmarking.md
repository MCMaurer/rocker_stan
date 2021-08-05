
# Benchmarking Stan speed

Quick semi-realistic test of speed running `brms` models in RStudio on various platforms.


## Platforms

`gpu06`: a speedy 256-core Ubuntu node
`local`: non-Docker on 16" MacBook Pro 2.4 GHz 8-Core Intel Core i9 32GB RAM
`VICE`: CyVerse VICE cloud, set with 8 cores, 16GB RAM, 64GB scratch

**NOTE** I ran `local` while doing other stuff on my laptop (writing some code, watching a video), which is, for my money, the most honest comparison to cloud-based options, since I can also do other stuff while I'm running models remotely.

## Code

```{r}
library(brms)
library(tidyverse)

install.packages("microbenchmark")


microbenchmark::microbenchmark({
  m1 <- brm(wind ~ year + (1 | status), data = storms, chains = 4, cores = 4)
},
times = 10)
```

## Run times (seconds)

|  method  |  min  |  lq  |  mean  |  median  |  uq  |  max  |
|----------|-------|------|--------|----------|------|-------|
| gpu06    |376.9|390.5|427.7|417.2|459.9|503.1|
| local    |482.9|590.4|646.1|629.4|703.6|846.0|
| VICE     |591.0|647.5|669.3|650.8|706.7|737.5|

## Conclusions

`gpu06` wins decisively, but my fairly beefy MacBook Pro was really not that much faster than VICE, and actually had significantly higher variance in speed (probably due to uneven loads on my computer while I was doing other tasks). I think this is a pretty compelling case for using VICE.