# STM32 电控培训作业（第一阶段）

本仓库用于存放电控组第一阶段培训的作业代码。

## 环境

- 开发板：STM32F103C8T6 最小系统板
- 配置工具：STM32CubeMX 6.18.1
- 编译/烧录：Keil MDK 5.43a + ST-Link V2
- 版本管理：Git

## 作业清单

### 基础部分（必做）

- [x] 1. 外接 LED 以 500ms 周期闪烁（使用 `HAL_Delay`）
- [x] 2. 外接 LED 以 500ms 周期闪烁（不使用 `HAL_Delay`）
- [x] 3. 按键通过 EXTI 中断控制 LED 开关，含 20ms 软件消抖
- [x] 4. 注册 GitHub 并把代码提交到本仓库

### 进阶部分（选做）

- [ ] 5. 短按（<1s）切换常亮/常灭；长按（≥1s）松手后 5Hz 快闪，再次长按退出
- [ ] 6. 双击（间隔 <500ms）翻转 LED，单击不动作

## 目录

| 目录 | 内容 |
| --- | --- |
| `01_blink_delay/` | 作业 1：HAL_Delay 闪烁 |
| `02_blink_timer/` | 作业 2：SysTick 非阻塞闪烁 |
| `03_exti_key/` | 作业 3：EXTI 按键 + 20ms 软件消抖 |

## 实现要点

- **作业 1**：`HAL_GPIO_TogglePin` + `HAL_Delay(500)`，阻塞式延时。
- **作业 2**：SysTick 中断里累加 `g_ms`，主循环比较 `now - last >= 500` 后翻转，不使用 `HAL_Delay`，也不调用 `HAL_GetTick`。
- **作业 3**：PA1 配置为 EXTI 下降沿 + 内部上拉，`HAL_GPIO_EXTI_Callback` 中用 20ms 时间窗 + 电平复核消抖，每按一次翻转 PA0 上的 LED。

## 硬件连接

| 功能 | 引脚 |
| --- | --- |
| LED（经限流电阻） | PA0 → 电阻 → LED 长脚，LED 短脚 → GND |
| 按键 | PA1，另一端接 GND（内部上拉） |

## 说明

每个作业是独立的 Keil 工程（`.uvprojx`），可直接用 Keil 打开、编译、烧录。
