
# 测试页面

本文档用于测试Github Markdown的各种效果。


显示SVG图像测试！

![](d9999.svg)

为什么SVG的图像不同步更新？！

这是代码区块
  #include "utils/lsyscache.h"
  #include "utils/rel.h"

PG_MODULE_MAGIC;

PG_FUNCTION_INFO_V1(pg_prewarm);

typedef enum
{
        PREWARM_PREFETCH,
        PREWARM_READ,
        PREWARM_BUFFER
} PrewarmType;


