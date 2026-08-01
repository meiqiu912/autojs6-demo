# AutoJs6 看剧任务自动化脚本 Demo v1.0

一个基于 AutoJs6 的看剧类 App 自动化任务脚本 Demo，解决"看剧领奖励"类 App 的重复手工操作：

## 功能
1. **自动看广告领奖励**：自动点击广告入口 → 等待播放 → 自动点击关闭/跳过
2. **自动看剧任务**：自动进入播放 → 模拟观看 15 秒 → 自动退出
3. **防风控**：操作间隔随机化 2-8 秒 + 滑动轨迹分段模拟人工（12 步带弧度）

## 运行环境
- Android 9+ 或 MuMu 模拟器
- AutoJs6 App 内运行（需开启无障碍服务）

## Demo 代码片段
```javascript
auto.waitFor(); // 等待无障碍服务授权

const config = {
  minInterval: 2000,   // 最小操作间隔(ms)
  maxInterval: 8000,   // 最大操作间隔(ms)
  adCloseTexts: ['关闭', '跳过', 'x', 'X', '×', '我知道了', '跳过广告'],
  taskTexts: ['任务', '任务中心', '活动中心'],
  watchTexts: ['看剧', '去观看', '立即观看', '开始播放'],
  adRewardTexts: ['看广告', '免费领取', '双倍奖励', '领奖励', '广告奖励'],
  watchSeconds: 15,    // 单次模拟观看时长(秒)
  maxLoops: 50,        // 总循环次数上限
};

/** 随机休眠（2-8秒防风控核心） */
function randomSleep(min = config.minInterval, max = config.maxInterval) {
  const ms = Math.floor(Math.random() * (max - min + 1)) + min;
  toast('等待 ' + (ms / 1000).toFixed(1) + 's');
  sleep(ms);
}

/** 模拟人工滑动（带弧度轨迹，防风控） */
function humanSwipe(x1, y1, x2, y2, totalDuration = 800) {
  const steps = 12;
  const dx = x2 - x1, dy = y2 - y1;
  for (let i = 1; i <= steps; i++) {
    const t = i / steps;
    const offset = Math.sin(t * Math.PI) * 40; // 弧线偏移
    swipe(x1 + dx * t + offset, y1 + dy * t, x2, y2, totalDuration / steps);
    sleep(50);
  }
}
// ... 完整版（自动找任务入口、看广告、领奖励、循环调度）接单后交付
```

## 接单说明
我是**白洁接单工作室**，承接 AutoJs6 / 自动化脚本定制：
- 按目标 App 实际控件树微调文案与坐标
- 打包独立 APK（apktool / 打包服务）
- 交付完整源码 + 使用说明
- 预算 200-500 元/单，可先 demo 验证再付全款，长期合作可谈

联系：QQ 2996053477 / 微信 xuchenai1319 / 邮箱 baijie.order@gmail.com
也接：Excel数据处理、简历优化、PPT大纲、文案代写（30-200元/单）。