---
categories:
  - "[[Skyrim]]"
modified: 2026-11-19T21:49:16.000Z
status: Complete
tags:
  - project/mutagen
urls:
  - https://github.com/Mutagen-Modding/Mutagen/pull/707
---
# Baseline
NOTE: `Prep` is not directly comparable, as some of its work was converted to lazy, and will instead by amortized across calls to `GetSpeakers`.

One benchmark for each condition function that filters run with `Skyrim.esm` loaded plus:
- `NonFilteringCondition`: Extra condition that does not filter, such as `GetStage`.
- `InvertedCondition`: `GetIsId` used to negate, benchmarking performance of inversion logic.
- `GetAllSpeakers`/`GetAllPaths`: Get all speakers/file paths for every response. gives an overview of overall performance.

| Method                |              Mean |           Error |         StdDev |         Gen0 |        Gen1 |    Gen2 |      Allocated |
| --------------------- | ----------------: | --------------: | -------------: | -----------: | ----------: | ------: | -------------: |
| Prep                  |    687,873.293 us |     884.0426 us |    826.9340 us |   48000.0000 |   7000.0000 |       - |   816159.83 KB |
| GetIsId               |          2.877 us |       0.0029 us |      0.0024 us |       0.3700 |      0.0038 |       - |        6.05 KB |
| GetIsVoiceType        |        123.191 us |       0.1562 us |      0.1461 us |       5.1270 |      0.7324 |       - |       86.04 KB |
| GetIsVoiceTypeList    |      5,934.585 us |       5.7906 us |      5.1332 us |     109.3750 |      7.8125 |       - |     1935.76 KB |
| GetIsAliasRef         |          4.861 us |       0.0055 us |      0.0048 us |       0.3891 |           - |       - |        6.43 KB |
| GetInFaction          |        244.219 us |       0.6291 us |      0.5885 us |      37.1094 |      9.7656 |       - |      607.96 KB |
| GetIsClass            |         13.205 us |  0.01 out 40 us |      0.0131 us |       1.8311 |      0.0610 |       - |       30.06 KB |
| HasKeyword            |          9.285 us |       0.0123 us |      0.0109 us |       0.9460 |      0.0153 |       - |       15.68 KB |
| GetIsRace             |        761.022 us |       1.4777 us |      1.2339 us |     107.4219 |     38.0859 |       - |     1759.41 KB |
| GetIsSex              |      1,907.222 us |      10.3582 us |      8.6495 us |     195.3125 |     85.9375 | 11.7188 |     3480.56 KB |
| IsInList              |         12.289 us |       0.0554 us |      0.0491 us |       1.1292 |      0.0153 |       - |        18.6 KB |
| IsChild               |         19.461 us |       0.0230 us |      0.0204 us |       2.2583 |      0.0610 |       - |       36.96 KB |
| NonFilteringCondition |          9.513 us |       0.0095 us |      0.0079 us |       0.4425 |           - |       - |        7.29 KB |
| InvertedCondition     |      1,067.862 us |       1.2497 us |      1.1078 us |     183.5938 |     78.1250 |       - |     3007.52 KB |
| SceneResponse         |         10.205 us |       0.0193 us |      0.0181 us |       0.3357 |           - |       - |        5.55 KB |
| GetAllSpeakers        | 10,221,645.860 us |  56,655.5368 us | 14,713.2630 us | 1033000.0000 | 451000.0000 |       - | 17149538.31 KB |
| GetAllPaths           |  9,532,661.220 us | 172,753.1566 us | 44,863.4463 us | 1018000.0000 | 418000.0000 |       - | 16827751.53 KB |
# VoiceSpeakers Lookup
Costs a bit on prep (within rounding errors), but greatly accelerates `GetIsVoiceType` searches.

| Method                |             Mean |           Error |          StdDev |        Gen0 |        Gen1 |    Gen2 |      Allocated |
| --------------------- | ---------------: | --------------: | --------------: | ----------: | ----------: | ------: | -------------: |
| Prep                  |   694,178.267 us |   1,479.1189 us |   1,383.5688 us |  48000.0000 |   7000.0000 |       - |   816319.45 KB |
| GetIsId               |         2.894 us |       0.0040 us |       0.0038 us |      0.3662 |           - |       - |        6.01 KB |
| GetIsVoiceType        |        35.333 us |       0.1451 us |       0.1357 us |      5.1880 |      0.5493 |       - |       85.76 KB |
| GetIsVoiceTypeList    |     2,502.021 us |       7.2278 us |       6.4073 us |    109.3750 |      7.8125 |       - |     1923.85 KB |
| GetIsAliasRef         |         4.880 us |       0.0031 us |       0.0029 us |      0.3891 |           - |       - |        6.38 KB |
| GetInFaction          |       248.321 us |       0.4348 us |       0.3631 us |     37.1094 |      9.2773 |       - |      606.98 KB |
| GetIsClass            |        13.149 us |       0.0202 us |       0.0179 us |      1.8005 |      0.0610 |       - |       29.45 KB |
| HasKeyword            |         9.304 us |       0.0129 us |       0.0107 us |      0.9308 |      0.0153 |       - |       15.35 KB |
| GetIsRace             |       777.302 us |       0.6308 us |       0.5592 us |    107.4219 |     37.1094 |       - |     1756.78 KB |
| GetIsSex              |     2,021.632 us |       3.4398 us |       3.2176 us |    195.3125 |     89.8438 | 11.7188 |     3475.33 KB |
| IsInList              |        12.123 us |       0.0316 us |       0.0295 us |      1.1139 |      0.0153 |       - |       18.42 KB |
| IsChild               |        19.379 us |       0.0301 us |       0.0282 us |      2.2278 |      0.0610 |       - |       36.81 KB |
| NonFilteringCondition |         9.937 us |       0.0347 us |       0.0290 us |      0.4425 |           - |       - |        7.24 KB |
| InvertedCondition     |       882.585 us |       2.2051 us |       1.7216 us |    149.4141 |     54.6875 |       - |      2445.3 KB |
| SceneResponse         |        10.725 us |       0.0815 us |       0.0762 us |      0.3357 |           - |       - |         5.5 KB |
| GetAllSpeakers        | 8,770,475.260 us | 590,819.5960 us | 153,433.9732 us | 901000.0000 | 411000.0000 |       - | 14987652.59 KB |
| GetAllPaths           | 8,662,795.200 us |  62,440.8869 us |  16,215.7001 us | 889000.0000 | 402000.0000 |       - | 14716489.96 KB |

# Remove Unscoped Resolve
Replace unscoped resolve in `GetIsVoiceType` form list check with HashSet lookup. Probably intended to check the entry was a voice type but never enforced this.

| Method             |             Mean |           Error |         StdDev |        Gen0 |        Gen1 |   Gen2 |      Allocated |
| ------------------ | ---------------: | --------------: | -------------: | ----------: | ----------: | -----: | -------------: |
| GetIsVoiceType     |        34.762 us |       0.0891 us |      0.0834 us |      5.1880 |      0.5493 |      - |       85.76 KB |
| GetIsVoiceTypeList |       413.706 us |       0.5566 us |      0.4648 us |     24.4141 |      6.3477 | 3.4180 |      470.92 KB |
| GetAllSpeakers     | 5,726,818.080 us |  29,877.3155 us |  7,759.0440 us | 801000.0000 | 384000.0000 |      - | 13352675.13 KB |
| GetAllPaths        | 5,593,401.680 us | 248,281.9038 us | 64,478.0221 us | 782000.0000 | 379000.0000 |      - | 12966821.27 KB |
# Null As Empty
Extract `GetConditionDataVoices`. Remove redundant creation of container that is normally overwritten. Slight reduction in allocations across the board.

| Method                |             Mean |           Error |         StdDev |        Gen0 |        Gen1 |    Gen2 |      Allocated |
| --------------------- | ---------------: | --------------: | -------------: | ----------: | ----------: | ------: | -------------: |
| Prep                  |   678,117.457 us |   2,012.1510 us |  1,783.7195 us |  48000.0000 |   7000.0000 |       - |   816412.98 KB |
| GetIsId               |         2.859 us |       0.0048 us |      0.0045 us |      0.3510 |           - |       - |        5.79 KB |
| GetIsVoiceType        |        34.409 us |       0.0683 us |      0.0639 us |      5.1880 |      0.0610 |       - |       85.54 KB |
| GetIsVoiceTypeList    |       414.214 us |       1.0115 us |      0.8967 us |     24.4141 |      7.3242 |  3.4180 |      470.71 KB |
| GetIsAliasRef         |         4.805 us |       0.0050 us |      0.0042 us |      0.3738 |           - |       - |        6.16 KB |
| GetInFaction          |       243.850 us |       0.6791 us |      0.5671 us |     37.1094 |     11.2305 |       - |      606.76 KB |
| GetIsClass            |        13.030 us |       0.0170 us |      0.0159 us |      1.7853 |      0.0610 |       - |       29.23 KB |
| HasKeyword            |         9.239 us |       0.0073 us |      0.0065 us |      0.9155 |      0.0153 |       - |       15.13 KB |
| GetIsRace             |       769.189 us |       0.8404 us |      0.7450 us |    107.4219 |     37.1094 |       - |     1756.56 KB |
| GetIsSex              |     1,936.550 us |       0.8315 us |      0.6943 us |    199.2188 |     85.9375 | 13.6719 |     3475.11 KB |
| IsInList              |        12.091 us |       0.0203 us |      0.0190 us |      1.0986 |      0.0153 |       - |        18.2 KB |
| IsChild               |        19.258 us |       0.0480 us |      0.0426 us |      2.2278 |      0.0610 |       - |        36.6 KB |
| NonFilteringCondition |         9.499 us |       0.0126 us |      0.0118 us |      0.4120 |           - |       - |         6.8 KB |
| InvertedCondition     |       830.362 us |       1.9207 us |      1.7966 us |    149.4141 |     55.6641 |       - |     2444.86 KB |
| SceneResponse         |        10.229 us |       0.0052 us |      0.0046 us |      0.3357 |           - |       - |         5.5 KB |
| GetAllSpeakers        | 5,882,939.050 us | 105,790.4969 us | 16,371.1965 us | 800000.0000 | 371000.0000 |       - | 13338488.83 KB |
| GetAllPaths           | 5,583,879.550 us | 137,333.9240 us | 21,252.5767 us | 781000.0000 | 369000.0000 |       - | 12951873.89 KB |
# Fewer Dictionaries
Reduce usage of `ToDictionary` in condition handling. Slight speedup to these conditions.

| Method         |             Mean |           Error |         StdDev |        Gen0 |        Gen1 |   Gen2 |      Allocated |
| -------------- | ---------------: | --------------: | -------------: | ----------: | ----------: | -----: | -------------: |
| GetInFaction   |       228.501 us |       1.7578 us |      1.6443 us |     29.7852 |      8.5449 |      - |      488.71 KB |
| GetIsClass     |        12.593 us |       0.0717 us |      0.0670 us |      1.4954 |      0.0458 |      - |       24.57 KB |
| HasKeyword     |         9.453 us |       0.0493 us |      0.0461 us |      0.8087 |           - |      - |       13.38 KB |
| GetIsRace      |       718.738 us |      10.6705 us |      9.9812 us |     87.8906 |     30.2734 |      - |     1451.15 KB |
| GetIsSex       |     1,628.995 us |      17.7351 us |     16.5894 us |    160.1563 |     64.4531 | 3.9063 |     2708.74 KB |
| IsChild        |        18.888 us |       0.2374 us |      0.1982 us |      1.6785 |      0.0305 |      - |       27.91 KB |
| GetAllSpeakers | 5,646,418.540 us | 243,152.9334 us | 63,146.0448 us | 778000.0000 | 340000.0000 |      - | 12797512.49 KB |
| GetAllPaths    | 5,563,053.750 us |  14,843.9176 us |  2,297.1127 us | 758000.0000 | 367000.0000 |      - | 12408120.21 KB |
# Faster Invert
Significantly improves invert performance, 2x whole load order speed from this alone.

| Method            |             Mean |           Error |         StdDev |        Gen0 |       Gen1 |     Allocated |
| ----------------- | ---------------: | --------------: | -------------: | ----------: | ---------: | ------------: |
| InvertedCondition |       136.203 us |       0.5975 us |      0.5296 us |     35.4004 |     8.7891 |     581.39 KB |
| GetAllSpeakers    | 2,462,293.150 us | 445,792.3865 us | 68,986.8652 us | 296000.0000 | 77000.0000 | 4921905.55 KB |
| GetAllPaths       | 2,164,193.480 us |  66,051.4975 us | 17,153.3642 us | 273000.0000 | 51000.0000 | 4481142.86 KB |
# Avoid Duplicate Work
 The `voiceContainers.Any()` check meant evaluating the container creation twice for each OR block. Significant improvements across the board from this.

| Method                |             Mean |          Error |        StdDev |        Gen0 |       Gen1 |   Gen2 |     Allocated |
| --------------------- | ---------------: | -------------: | ------------: | ----------: | ---------: | -----: | ------------: |
| GetIsId               |         2.577 us |      0.0044 us |     0.0041 us |      0.2823 |          - |      - |       4.65 KB |
| GetIsVoiceType        |        32.898 us |      0.0920 us |     0.0860 us |      5.0659 |     0.6104 |      - |      83.07 KB |
| GetIsVoiceTypeList    |       413.151 us |      3.3923 us |     3.1732 us |     22.9492 |     7.3242 | 3.4180 |     447.93 KB |
| GetIsAliasRef         |         4.497 us |      0.0028 us |     0.0023 us |      0.2975 |          - |      - |       4.93 KB |
| GetInFaction          |       142.672 us |      0.3781 us |     0.3352 us |     17.5781 |     4.1504 |      - |     287.95 KB |
| GetIsClass            |         9.648 us |      0.0108 us |     0.0096 us |      0.9613 |     0.0153 |      - |      15.92 KB |
| HasKeyword            |         7.727 us |      0.0089 us |     0.0079 us |      0.5341 |          - |      - |       8.91 KB |
| GetIsRace             |       438.423 us |      0.7494 us |     0.6643 us |     50.2930 |    18.5547 |      - |     822.82 KB |
| GetIsSex              |     1,041.801 us |     14.4222 us |    13.4905 us |     91.7969 |    35.1563 | 3.9063 |    1569.08 KB |
| IsInList              |         9.976 us |      0.0095 us |     0.0089 us |      0.6256 |          - |      - |      10.26 KB |
| IsChild               |        14.334 us |      0.0364 us |     0.0323 us |      1.1749 |     0.0305 |      - |      19.41 KB |
| NonFilteringCondition |         9.310 us |      0.0056 us |     0.0047 us |      0.3357 |          - |      - |       5.59 KB |
| InvertedCondition     |        92.716 us |      0.2559 us |     0.2394 us |     21.3623 |     4.2725 |      - |     349.46 KB |
| SceneResponse         |        10.325 us |      0.0092 us |     0.0086 us |      0.3357 |          - |      - |        5.5 KB |
| GetAllSpeakers        | 1,393,331.080 us | 10,210.7890 us | 2,651.7095 us | 128000.0000 | 41000.0000 |      - | 2180582.01 KB |
| GetAllPaths           | 1,097,460.240 us |  5,469.1505 us | 1,420.3210 us |  92000.0000 | 26000.0000 |      - | 1528851.06 KB |
# Iterate Traits Once
Gives decent speedup to prep

| Method |           Mean |         Error |        StdDev |       Gen0 |      Gen1 |    Allocated |
| ------ | -------------: | ------------: | ------------: | ---------: | --------: | -----------: |
| Prep   | 481,401.033 us | 1,354.7557 us | 1,267.2393 us | 32000.0000 | 5000.0000 | 549575.29 KB |
# Usage Cache
Use usage cache for scene responses. Saves memory at the cost of minimal performance loss in scenes.

| Method         |             Mean |          Error |        StdDev |        Gen0 |       Gen1 |     Allocated |
| -------------- | ---------------: | -------------: | ------------: | ----------: | ---------: | ------------: |
| Prep           |   415,272.464 us |  1,209.7153 us | 1,072.3811 us |  28000.0000 |  4000.0000 |  479801.68 KB |
| SceneResponse  |        11.487 us |      0.0101 us |     0.0095 us |      0.3967 |          - |       6.66 KB |
| GetAllSpeakers | 1,396,834.575 us | 21,825.2363 us | 3,377.4795 us | 129000.0000 | 43000.0000 | 2190557.86 KB |
| GetAllPaths    | 1,104,135.420 us | 10,095.1296 us | 2,621.6731 us |  93000.0000 | 26000.0000 | 1539280.68 KB |
# Remove IsDefault
Represent universal set as null instead. Getting into micro optimisations here, but still gives multiple std deviations speedup and decent reduction in allocations.

| Method                |             Mean |          Error |        StdDev |        Gen0 |       Gen1 |   Gen2 |     Allocated |
| --------------------- | ---------------: | -------------: | ------------: | ----------: | ---------: | -----: | ------------: |
| Prep                  |   429,702.377 us |  1,315.5164 us | 1,098.5158 us |  29000.0000 |  5000.0000 |      - |  485437.54 KB |
| GetIsId               |         2.477 us |      0.0027 us |     0.0023 us |      0.2480 |          - |      - |       4.06 KB |
| GetIsVoiceType        |        32.129 us |      0.0607 us |     0.0538 us |      5.0049 |     0.5493 |      - |      82.48 KB |
| GetIsVoiceTypeList    |       406.839 us |      0.4983 us |     0.4417 us |     22.4609 |     6.3477 | 3.4180 |     438.29 KB |
| GetIsAliasRef         |         4.385 us |      0.0051 us |     0.0048 us |      0.2594 |          - |      - |       4.34 KB |
| GetInFaction          |       111.406 us |      0.2713 us |     0.2538 us |     11.3525 |     2.1973 |      - |     186.98 KB |
| GetIsClass            |         8.427 us |      0.0083 us |     0.0074 us |      0.7019 |          - |      - |      11.57 KB |
| HasKeyword            |         7.086 us |      0.0059 us |     0.0049 us |      0.4044 |          - |      - |       6.66 KB |
| GetIsRace             |       329.743 us |      1.4947 us |     1.3982 us |     30.7617 |     9.2773 |      - |     508.65 KB |
| GetIsSex              |       800.213 us |      1.9596 us |     1.8330 us |     56.6406 |    23.4375 | 3.9063 |     998.57 KB |
| IsInList              |         9.148 us |      0.0098 us |     0.0086 us |      0.4120 |          - |      - |       6.74 KB |
| IsChild               |        13.031 us |      0.0105 us |     0.0093 us |      0.9155 |     0.0153 |      - |      15.14 KB |
| NonFilteringCondition |         9.052 us |      0.0065 us |     0.0061 us |      0.3052 |          - |      - |          5 KB |
| InvertedCondition     |        82.308 us |      0.1407 us |     0.1316 us |     17.5781 |     4.3945 |      - |     288.66 KB |
| SceneResponse         |        11.366 us |      0.0138 us |     0.0122 us |      0.3662 |          - |      - |       6.08 KB |
| GetAllSpeakers        | 1,355,111.600 us | 20,700.3245 us | 5,375.8085 us | 117000.0000 | 40000.0000 |      - | 2004564.71 KB |
| GetAllPaths           | 1,079,140.720 us |  9,834.9540 us | 2,554.1063 us |  87000.0000 | 26000.0000 |      - |  1436868.8 KB |
# Fix Traits Inheritance
| Method                |             Mean |          Error |         StdDev |        Gen0 |       Gen1 |   Gen2 |     Allocated |
| --------------------- | ---------------: | -------------: | -------------: | ----------: | ---------: | -----: | ------------: |
| Prep                  |   427,597.964 us |  1,745.2869 us |  1,547.1514 us |  29000.0000 |  6000.0000 |      - |  482930.77 KB |
| GetIsId               |         2.498 us |      0.0024 us |      0.0023 us |      0.2480 |          - |      - |       4.07 KB |
| GetIsVoiceType        |        32.260 us |      0.0452 us |      0.0423 us |      5.0049 |     0.5493 |      - |      82.49 KB |
| GetIsVoiceTypeList    |       406.745 us |      1.3656 us |      1.1403 us |     22.4609 |     6.8359 | 3.4180 |      438.3 KB |
| GetIsAliasRef         |         4.405 us |      0.0032 us |      0.0029 us |      0.2594 |          - |      - |       4.35 KB |
| GetInFaction          |       109.811 us |      0.2855 us |      0.2670 us |     11.3525 |     2.1973 |      - |     186.99 KB |
| GetIsClass            |         8.489 us |      0.0051 us |      0.0045 us |      0.7019 |          - |      - |      11.58 KB |
| HasKeyword            |         6.483 us |      0.0061 us |      0.0057 us |      0.2747 |          - |      - |       4.55 KB |
| GetIsRace             |       125.809 us |      0.4727 us |      0.4421 us |     11.7188 |     2.1973 |      - |     194.22 KB |
| GetIsSex              |       360.766 us |      4.0567 us |      3.7946 us |     21.4844 |     4.8828 | 3.4180 |     429.06 KB |
| IsInList              |         9.191 us |      0.0080 us |      0.0071 us |      0.4120 |          - |      - |       6.75 KB |
| IsChild               |        13.140 us |      0.0126 us |      0.0112 us |      0.9155 |     0.0153 |      - |      15.15 KB |
| NonFilteringCondition |         9.140 us |      0.0111 us |      0.0099 us |      0.3052 |          - |      - |       5.01 KB |
| InvertedCondition     |        80.007 us |      0.2050 us |      0.1918 us |     17.5781 |     4.3945 |      - |     288.68 KB |
| SceneResponse         |        11.525 us |      0.0225 us |      0.0210 us |      0.3662 |          - |      - |       6.09 KB |
| GetAllSpeakers        | 1,155,486.820 us | 15,140.0263 us |  3,931.8168 us | 100000.0000 | 28000.0000 |      - | 1719539.15 KB |
| GetAllPaths           |   998,582.275 us | 68,920.5517 us | 10,665.5316 us |  73000.0000 | 15000.0000 |      - | 1211822.07 KB |
