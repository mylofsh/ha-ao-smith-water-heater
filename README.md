# AI-LiNK Water Heater

Home Assistant 集成：控制 A.O.史密斯 AI-LiNK 智慧家燃气热水器。

## 功能

| 平台 | 实体 |
|---|---|
| `water_heater` | 开关、目标水温、当前工作状态 |
| `sensor` | 进水温度、出水温度、水流、故障码等 |
| `switch` | 巡航、增压等功能开关 |
| `number` | 巡航保温时长等可调参数 |

## 安装

### HACS（推荐）

1. HACS → 集成 → 右上角菜单 → **自定义存储库**
2. 填入 `https://github.com/mylofsh/ha-ailink-heater`，类型选 **Integration**
3. 搜索「AI-LiNK Water Heater」安装
4. **重启 Home Assistant**

### 手动

把 `custom_components/ailink_water_heater` 整个目录复制到 HA 的
`config/custom_components/` 下，然后重启。

## 配置

**设置 → 设备与服务 → 添加集成 → 搜索「AI-LiNK」**

需要四项凭据（从抓包或 App 获取）：

| 字段 | 说明 |
|---|---|
| Token | 访问令牌 |
| User ID | 用户标识 |
| Family ID | 家庭标识 |
| Device ID | 设备标识 |

请求头 `source: Web`，走 H5 Web 路径（该路径不校验 `sign` / `md5data`）。

## 更新令牌（重要）

访问令牌会过期。**令牌失效后无需删除并重新添加集成**，直接更新即可：

> **设置 → 设备与服务 → AI-LiNK 燃气热水器 → 配置 → 选项**

填入新的 Token，保存后集成自动重载并立即使用新令牌。

- 留空的字段表示**保持原值不变**（只改 Token 时，其他三项不用动）
- 保存前会校验新凭据，校验失败提示 `认证失败`，原配置不受影响

## 实现说明

- 状态轮询：`POST /AiLinkService/appDevice/getDeviceCurrInfo`
- 设备控制：`POST /AiLinkService/device/invokeMethod`
- 默认轮询间隔 60 秒，可在选项中以 10–600 秒调整

## 故障排查

| 现象 | 处理 |
|---|---|
| 提示 `认证失败` | Token 已过期，去 **配置 → 选项** 更新 Token |
| 实体状态不更新 | 检查轮询间隔，或看 HA 日志中的 `UpdateFailed` 详情 |

## 许可

MIT
