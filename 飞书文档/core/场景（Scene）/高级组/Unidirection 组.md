# Unidirection 组

## Unidirection 组
## @黄强盛@丁禹伯

G-MAPF 不支持此功能，可使用 G-MAPF 的 fluent 属性实现类似效果
Unidirection 组是单向互斥区组，是一个图元的组合，包含线路，点、区块。当多机器人从相同入口进入Unidirection组时，Unidirection 可以允许所有机器人进入。当多机器人从不同的入口进入Unidirection组时，Unidireciton只允许一台机器人进入。对于已经在Unidirection 组内的机器人，始终允许它在Unidirection 组移动。
1670903736482-2c5bf767-c919-4283-9434-d7f7a1270c6b.png

1670903885992-5b6a9210-3926-4b5e-8d62-e8d49530c093.gif

特别注意的是 ，已经在Unidirection组内的机器人，可以去往任意方向。因此，当机器人在 Unidirection 组内被取消任务后，再重新规划新的任务，可能会和已经在Unidirection组内的机器人发生死锁。
