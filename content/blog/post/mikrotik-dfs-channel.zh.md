---
title: Mac 偶尔识别不到 MikroTik 的 Wi-Fi 的故障排查
date: 2026-11-17
description: M4 Mac 偶尔扫不到 MikroTik 的 5G SSID，重启也没用。最后发现是 DFS 信道的问题，路由器自动选频落进了 Mac 射频不支持的 100-140 段
tags: [wifi, 踩坑, 网络]
---

有个问题断断续续出现好久了：家里 MikroTik hAP ax² 的 5G Wi-Fi，有时候会在 M4 Mac 的列表里直接消失。重启 Mac 没用，重启路由器也没用，过段时间自己又回来了。不过手机和其他设备一直都能连，我也就一直没认真查。

这次能查出来其实是因为另外一件事。在调一个 M5Stack Cardputer 扫不到 Wi-Fi 的问题，发现是信道的问题，于是在今晚 Mac 再次突然断网并表现出扫描不到 Wi-Fi 的时候联想到了会不会也是同一个原因？

一查果然是。SSH 上路由器看了一眼：

```
/interface wifi monitor wifi1
channel: 5500/ax/Ceee/D
```

5500 MHz 就是信道 100，最后的 D 表示 DFS。而国区 Mac 的 5G 信道只支持 36-64 和 149-165（在 `system_profiler SPAirPortDataType` 的 Supported Channels 里能看到），100-140 的段不在射频范围内

## 跳到信道 100 的原因

5G 接口一直没固定 frequency，配置里有一行 `channel.skip-dfs-channels=10min-cac`，含义是「可以用 DFS 信道，只跳过需要 10 分钟 CAC 检测的那些」，52-64、100-140 这些普通 DFS 信道全都在候选池里，RouterOS 就挑中了 5500 这个刚好撞上了 Mac 不支持的区间。

## MikroTik 什么时候会换信道

RouterOS 的自动选频只在接口启动时做一次，不像有些家用路由会在后台一直扫频找什么「更优」信道，更换时机如下：

- 重启或者接口重新 enable，重选一遍，结果可能和上次完全不同
- DFS 信道上检测到雷达，法规要求几秒内撤离，然后重新选频。原信道还会进入大概 30 分钟的 non-occupancy 期，不能再用
- 手动改了 channel 相关的配置

(以下为 LLM 补充部分):

还有一层：选到 DFS 信道之后，接口得先静默监听雷达 60 秒才开始发 beacon，这叫 CAC，天气雷达段（120-128 附近）更是要 10 分钟。也就是说，就算最后落在了一个能用的信道上，中间也有一分钟 SSID 在空中根本不存在。要是雷达误报再来一轮，又是一分钟起步。

这么看，「突然消失，重启后还是没有」就完全说得通了：大概率是雷达事件或者带雷达特征的信号触发了撤离，重新选频又刚好落进 100-140，Mac 自然就再也看不见了。手机能看到，是因为不同设备的区域码和射频支持不一样。

## 修法

就一行：

```
/interface wifi set 0 channel.skip-dfs-channels=all
```

改完自动重选频，落到信道 48，Mac 就能扫到了

想更稳的话，可以直接用 `channel.frequency=` 钉死一个非 DFS 信道，比如 5785（信道 157），这样就不用每次启动的时候随机抽奖信道了
