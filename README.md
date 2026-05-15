# FakeGPS v5.0 - iPhone 虚拟定位工具

免越狱、无需 Xcode，通过 USB 为 iPhone 设置虚拟 GPS 定位。

## 功能特性

- **虚拟定位**：将 iPhone 定位修改到世界上任意位置
- **地图选点**：在高德地图上点击选点，自动转换坐标
- **一键设置**：网页端直接修改定位，无需切换终端
- **自定义地点**：添加常用位置为快捷命令
- **轨迹模拟**：支持 GPX 文件播放，模拟步行/驾车轨迹
- **坐标转换**：自动处理 GCJ-02（高德）→ WGS-84（GPS）坐标偏移

## 环境要求

- macOS（已测试 macOS Sonoma/Sequoia）
- iPhone（已测试 iOS 18/26，需连接数据线）
- Python 3.9+（macOS自带Python）

## 安装

### 方式一：npm（推荐）

```bash
npm install -g fakegps
```

安装完成后直接在终端运行：
```bash
fakegps
```

### 方式二：Homebrew

```bash
brew tap sixzjd/fakegps
brew install fakegps
```

### 方式三：手动安装

```bash
# 安装依赖
pip3 install pymobiledevice3

# 下载本项目
git clone https://github.com/sixzjd/fakeGPS-for-iPhone.git
cd fakegps
chmod +x fakegps

# 可选：添加到 PATH
sudo ln -s $(pwd)/fakegps /usr/local/bin/fakegps
```

## 快速开始

### 1. 启动隧道服务

iOS 17+ 需要先建立 USB 隧道，在**一个终端窗口**运行（保持不要关闭）：

```bash
sudo python3 -m pymobiledevice3 remote tunneld
```

### 2. 使用交互模式（推荐）

```bash
fakegps
```

进入交互模式后：
```
fakegps> tiananmen    # 定位到天安门
fakegps> /set 39.9 116.3  # 设置坐标
fakegps> /map         # 打开地图选点
fakegps> /clear       # 恢复真实定位
fakegps> /places      # 查看所有地点
fakegps> /exit        # 退出
```

### 3. 直接执行命令

```bash
fakegps tiananmen     # 直接定位到天安门
fakegps /set 39.9 116.3  # 设置坐标
fakegps /clear        # 恢复真实定位
fakegps /list         # 查看已连接设备
```

## 内置地点

| 名称 | 坐标 (WGS-84) | 说明 |
|------|---------------|------|
| tiananmen | 39.90776, 116.39565 | 天安门 |
| guomao | 39.90727, 116.45877 | 国贸CBD |
| wangjing | 39.99271, 116.47673 | 望京SOHO |
| shanghai | 31.22840, 121.47370 | 上海外滩 |
| shenzhen | 22.54600, 114.05790 | 深圳 |
| paris | 48.8584, 2.2945 | 巴黎埃菲尔铁塔 |
| newyork | 40.7580, -73.9855 | 纽约时代广场 |
| tokyo | 35.6586, 139.7454 | 东京塔 |
| london | 51.5074, -0.1278 | 伦敦大本钟 |

## 自定义地点管理

```bash
# 添加地点（名称 经纬度，支持高德坐标，自动转换）
fakegps /add myhome 39.9 116.3

# 使用自定义地点
fakegps myhome

# 查看所有地点（内置 + 自定义）
fakegps /places

# 删除自定义地点
fakegps /remove myhome
```

自定义地点保存在 `~/.fakegps_places` 文件中。

## 地图选点

```bash
fakegps /map
```

会打开浏览器地图界面，点击选点后复制命令到终端运行。

需要配置高德地图 API Key 才能使用搜索功能（在地图页面顶部输入）。

## 坐标系说明

中国境内地图存在坐标偏移问题：
- **高德/腾讯地图**：使用 GCJ-02 坐标系（有偏移）
- **iPhone GPS**：使用 WGS-84 坐标系（真实坐标）
- 两者在中国境内偏差 100-700 米

本工具自动判断坐标是否在中国境内：
- **中国境内**：自动将高德坐标 (GCJ-02) 转换为 GPS 坐标 (WGS-84)
- **中国境外**：直接使用原始坐标，无需转换

国外坐标示例：
```bash
fakegps /set 48.8584 2.2945    # 巴黎埃菲尔铁塔
fakegps /set 40.7580 -73.9855  # 纽约时代广场
fakegps /set 35.6762 139.6503  # 东京涩谷
fakegps /set 51.5074 -0.1278   # 伦敦大本钟
```

## 配置 AMap Key（地图搜索功能）

1. 访问 [高德开放平台](https://lbs.amap.com/) 注册账号
2. 创建应用并获取 Web 服务 API Key
3. 在地图页面顶部输入 Key 并保存

## 常见问题

**Q: 提示 "Unable to connect to Tunneld"**
A: 确保在单独的一个终端窗口中 `sudo python3 -m pymobiledevice3 remote tunneld` 正在运行。

**Q: iPhone 上没有"开发者模式"选项**
A: 连接 USB 后首次使用 pymobiledevice3 时，iPhone 会自动弹出开启提示。如果没有，重启 iPhone 后重试。

**Q: 定位不准确，有几百米偏差**
A: 确保使用的是最新版本，旧版本可能没有坐标转换功能。

**Q: 拔掉数据线后定位恢复了**
A: 这是正常行为，虚拟定位需要保持 USB 连接。重启 iPhone 也会恢复真实定位。

## npm 包

https://www.npmjs.com/package/fakegps

## 免责声明

本工具仅供学习和研究使用。使用者应遵守当地法律法规，不得将其用于任何违法违规用途。使用本工具所产生的一切后果由使用者自行承担。

## License

MIT
