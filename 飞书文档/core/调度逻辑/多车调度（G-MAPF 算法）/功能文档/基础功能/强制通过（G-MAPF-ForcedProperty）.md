# 强制通过（G-MAPF-ForcedProperty）

### 强制通过（G-MAPF-ForcedProperty）

## 参数名称

## 参数位置

## 默认值

## 支持版本

### G-MAPF-ForcedProperty

## 导航参数

## false

## ~lastest

强制通过点位的机器人尺寸缩放比例。

对于一些特殊场景，需要标记某些点位机器人必定可以通过时，可以使用 MAPFProperties 中的 forced 属性。上述导航参数可以控制强制通过的系数。默认为 0.99 ，即机器人的模型为实际大小的 99%，最小值可以配置为 0。当配置为 0 时，代表不做任何碰撞检测。
