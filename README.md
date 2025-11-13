# Clash Meta 配置文件说明

> 主要是自用，但是又不止一台设备需要，遂备份上来，怕后面太长时间不用忘了配置，遂写MD
> 优化的 Clash Meta 配置，支持社交媒体 IP 归属地控制、智能分流、DNS 防泄露

## 🌟 特性

- ✅ **社交媒体 IP 归属地控制** - 抖音/小红书/微博显示代理节点 IP
- ✅ **智能分流** - CDN 直连，API 走代理，节省流量
- ✅ **DNS 防泄露** - 完善的 DNS 配置，避免 DNS 污染
- ✅ **去广告** - 集成广告拦截规则
- ✅ **模块化规则** - 规则分类清晰，易于维护
- ✅ **适配 akashaproxy** - 完美支持安卓透明代理

## 🚀 快速开始

### 1. 直接Download
```
Code -> Download
```

### 2. 配置订阅链接
编辑 `config.yaml`，找到 `proxy-providers` 部分：
```yaml
proxy-providers:
  Name:
    url: "在这里填写你的订阅链接"  # 替换为实际订阅
    path: ./proxies/Name.yaml
```

### 3. 上传到设备
- **安卓**：上传到 `/data/clash/`
- **电脑**：放到 Clash Meta 配置目录

### 4. 重启服务
```bash
# akashaproxy
/data/clash/scripts/clash.tool -r
```

## 📁 文件结构

```
/data/clash/
├── config.yaml              # 主配置文件
├── clash.config             # akashaproxy 配置
├── proxies/                 # 代理订阅文件
│   └── Name.yaml
├── rule_providers/          # 在线订阅规则
│   └── Ads.yaml            # 广告拦截规则
└── rules/                   # 本地自定义规则
    ├── social.yaml         # 社交媒体规则
    ├── cdn-direct.yaml     # CDN 直连规则
    ├── ai.yaml             # AI 服务规则
    └── custom-direct.yaml  # 自定义直连规则
```

## 📝 规则文件说明

### 1. social.yaml - 社交媒体 IP 归属地控制
控制抖音、小红书、微博等应用显示的 IP 归属地。
- 只代理 API 接口（获取 IP 信息）
- CDN 内容走直连（视频/图片快速加载）

**编辑方法**：
```yaml
payload:
  - DOMAIN-SUFFIX,douyin.com    # 添加更多社交应用
  - DOMAIN-SUFFIX,kuaishou.com  # 例如：快手
```

### 2. cdn-direct.yaml - CDN 直连
社交媒体的 CDN 资源直连，避免代理浪费流量。

**编辑方法**：
```yaml
payload:
  - DOMAIN-SUFFIX,douyinvod.com  # 视频 CDN
  - DOMAIN-KEYWORD,cdn           # 关键词匹配
```

### 3. ai.yaml - AI 服务
OpenAI、Claude、Gemini、通义千问等 AI 服务。

**编辑方法**：
```yaml
payload:
  - DOMAIN-SUFFIX,coze.com       # 添加扣子
  - DOMAIN-SUFFIX,poe.com        # 添加 Poe
```

### 4. custom-direct.yaml - 自定义直连
机场订阅、个人常用网站等需要直连的域名。

**编辑方法**：
```yaml
payload:
  - DOMAIN-SUFFIX,example.com    # 添加你的域名
  - DOMAIN-KEYWORD,keyword       # 关键词匹配
```

## 🔧 修改后如何生效

1. **编辑规则文件**（如 `rules/social.yaml`）
2. **保存文件**
3. **重启 akashaproxy**
   ```bash
   /data/clash/scripts/clash.tool -r
   ```

## ✨ 优点

- ✅ 主配置文件更简洁（从 446 行减少到约 300 行）
- ✅ 规则分类清晰，易于查找
- ✅ 修改某类规则不影响其他配置
- ✅ 可以快速开关某类规则（注释 RULE-SET 即可）
- ✅ 便于备份和分享特定规则集

## 📌 注意事项

1. **规则顺序很重要**：
   - cdn-direct 必须在 social-media 之前
   - 所有自定义规则必须在 GEOSITE,cn 之前

2. **文件路径**：
   - 相对路径基于主配置文件位置
   - 确保 `rules/` 目录存在

3. **格式要求**：
   - 规则文件必须以 `payload:` 开头
   - 每条规则前面要有 `- ` 和空格

## 🆘 常见问题

**Q: 修改规则文件后不生效？**
A: 检查文件格式，确保每行前有 `- `，然后重启服务。

**Q: 想临时禁用某个规则集？**
A: 在主配置文件中注释对应的 `- RULE-SET` 行：
```yaml
# - RULE-SET,social-media,社交媒体  # 临时禁用
```

**Q: 如何添加新的规则集？**
A: 
1. 在 `rules/` 目录创建新文件
2. 在主配置 `rule-providers:` 中添加定义
3. 在 `rules:` 中添加引用

---

**配置优化时间**: 2025-11-13  
**Clash Meta 版本**: mihomo  

**代理工具**: akashaproxy  
