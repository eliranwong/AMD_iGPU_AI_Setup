# BIOS Memory Setting (reboot+DEL key):

UEFI/BIOS -> Advanced -> AMD CBS -> NBIO -> GFX Configuration > 

```
iGPU Advanced Control > Disabled
Dedicated Graphics Memory > Medium (16GB)
Remaining System Memory > 48GB
```

Different memory settings tried for Ollama, no significant difference.

# Speed Test with Ollama

Tested prompt: What is machine learning?

Model: `deepseek-r1:1.5b`

```
load duration:        12.277249ms
prompt eval count:    8 token(s)
prompt eval duration: 53ms
prompt eval rate:     150.94 tokens/s
eval count:           177 token(s)
eval duration:        3.808s
eval rate:            46.48 tokens/s
```

Model: `deepseek-r1:7b`

```
total duration:       35.692882833s
load duration:        24.075692ms
prompt eval count:    8 token(s)
prompt eval duration: 254ms
prompt eval rate:     31.50 tokens/s
eval count:           563 token(s)
eval duration:        35.412s
eval rate:            15.90 tokens/s
```

Model: `deepseek-r1:8b`

```
total duration:       19.19488017s
load duration:        19.389461ms
prompt eval count:    8 token(s)
prompt eval duration: 204ms
prompt eval rate:     39.22 tokens/s
eval count:           284 token(s)
eval duration:        18.969s
eval rate:            14.97 tokens/s
```

Model: `deepseek-r1:14b`

```
total duration:       1m29.657527203s
load duration:        19.170988ms
prompt eval count:    8 token(s)
prompt eval duration: 376ms
prompt eval rate:     21.28 tokens/s
eval count:           660 token(s)
eval duration:        1m29.261s
eval rate:            7.39 tokens/s
```

Model: `deepseek-r1:32b`

```
total duration:       5m53.281645538s
load duration:        18.26359ms
prompt eval count:    8 token(s)
prompt eval duration: 852ms
prompt eval rate:     9.39 tokens/s
eval count:           1308 token(s)
eval duration:        5m52.409s
eval rate:            3.71 tokens/s
```

Model: `deepseek-r1:70b`

```
total duration:       3m52.784148477s
load duration:        31.808434ms
prompt eval count:    8 token(s)
prompt eval duration: 2s
prompt eval rate:     4.00 tokens/s
eval count:           406 token(s)
eval duration:        3m50.749s
eval rate:            1.76 tokens/s
```
