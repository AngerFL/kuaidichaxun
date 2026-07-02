# kuaidichaxun（快递查询）

一个用于计算快递价格的 WorkBuddy Skill，支持智能地址解析、抛重计算、多渠道比价等功能。

## ✨ 功能特性

- 🗺️ **智能地址解析** - 支持省份、城市、别名，自动识别地址编码
- 📊 **精准价格计算** - 基于离线数据库，解析首重+续重规则
- 📦 **抛重计算** - 体积重量 vs 实际重量，取大值计费
- 🔄 **智能去重** - 同一快递公司只展示最便宜渠道
- 🌍 **完整地址库** - 34省份、300+城市、常见别名

## 🚀 快速开始

### 安装

1. 下载本仓库
2. 将 `kuaidichaxun` 文件夹放到 `~/.workbuddy/skills/` 目录
3. 重启 WorkBuddy

### 命令行使用

```bash
cd ~/.workbuddy/skills/kuaidichaxun
python scripts/calculate.py \
    --data references/all_channels_prices.json \
    --from 上海 \
    --to 扬州 \
    --weight 5
```

### 参数说明

| 参数 | 必填 | 说明 |
|------|------|------|
| `--data` | ✅ | 价格数据JSON文件 |
| `--from` | ✅ | 寄件地 |
| `--to` | ✅ | 收件地 |
| `--weight` | ✅ | 重量（kg） |
| `--length` | ❌ | 长度（cm） |
| `--width` | ❌ | 宽度（cm） |
| `--height` | ❌ | 高度（cm） |

## 📊 示例

### 基本查询

```bash
python scripts/calculate.py \
    --data references/all_channels_prices.json \
    --from 上海 \
    --to 扬州 \
    --weight 5
```

**输出**：
```
📊 计算结果（上海 → 扬州，5.0kg）

排名    渠道名称                预估价格（元）   价格规则                                    
================================================================================================
🥇1     韵达速递                 9.77           1-50公斤,价格4.08续1.20;
🥈2     圆通速递                 10.10          1-50公斤,价格4.78续1.1;
```

### 抛重计算

```bash
python scripts/calculate.py \
    --data references/all_channels_prices.json \
    --from 上海 \
    --to 北京 \
    --weight 2 \
    --length 40 \
    --width 30 \
    --height 20
```

**输出**：
```
📐 抛重计算：
- 实际重量：2.0kg
- 体积重量：3.0kg (40×30×20÷8000)
- 计费重量：3.0kg
```

## 📁 文件结构

```
kuaidichaxun/
├── SKILL.md                 # 技能文档
├── README.md                # 本文件
├── scripts/
│   └── calculate.py         # 核心计算脚本
├── references/
│   ├── all_channels_prices.json   # 价格数据库
│   └── address_mapping.json       # 地址编码映射
└── assets/
    └── promotion.png        # 推广图片（可选）
```

## 📝 在 WorkBuddy 中使用

安装后，可以在对话中直接使用：

```
用户：上海到扬州，5公斤，多少钱？
助手：[自动调用技能，展示价格结果]
```

## 🔧 高级用法

### 批量查询

```bash
for city in 南京 杭州 苏州; do
    python scripts/calculate.py \
        --data references/all_channels_prices.json \
        --from 上海 \
        --to $city \
        --weight 5
done
```

### 自定义抛重系数

```bash
python scripts/calculate.py \
    --data references/all_channels_prices.json \
    --from 上海 \
    --to 广州 \
    --weight 10 \
    --light-goods 6000
```

## 🛠️ 技术实现

- **地址解析**：省份编码 → 城市编码 → 名称 → 别名
- **价格计算**：解析规则字符串 → 匹配重量段 → 计算费用
- **抛重计算**：体积重量 = 长×宽×高÷抛重系数
- **同公司去重**：识别公司关键词 → 分组 → 保留最低价

## 📦 依赖

- Python 3.6+
- 标准库：json, re, sys, os

无需额外安装第三方库。

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📄 许可证

MIT License

## 🙏 致谢

- 地址编码数据：国家标准《中华人民共和国行政区划代码》
- 快递价格数据：各快递公司公开数据

## 📧 联系方式

如有问题或建议，请提交 Issue。

---

**⭐ 如果这个项目对你有帮助，请给它一个星标！**
