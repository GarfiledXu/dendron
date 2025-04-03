---
id: t8cjli0jh82tjv55hkxss16
title: Regular
desc: ''
updated: 1743156314369
created: 1743155380827
---

### 将 printf(%<>) 格式替换为 SLOG({})格式

```bash
printf\((.*?)(%d)(.*?)\)
SLOGE($1{}$3)
#能够将
printf("ERROR: Bind VI[0] and VENC[0] error! ret=%d\n", ret);
SLOGE("ERROR: Bind VI[0] and VENC[0] error! ret={}\n", ret);

#但还需要去除\n,以及不同%<>格式
#??? 如何单独去除 \n呢

```