# 🤖 Google One (Gemini) Verification Tool

Python tool for Google One AI Premium student discount via SheerID.

---

## 📋 Requirements

- Python 3.8+
- `httpx` - HTTP client
- `Pillow` - Image generation

---

## 🚀 Quick Start

### 1. Clone Repository

```bash
git clone https://github.com/ThanhNguyxn/SheerID-Verification-Tool.git
```

### 2. Go to Tool Directory

```bash
cd SheerID-Verification-Tool/one-verify-tool
```

### 3. Install Dependencies

```bash
pip install httpx Pillow
```

**[Optional] Enhanced Anti-Detection:**
```bash
pip install curl_cffi cloudscraper
```
> `curl_cffi` spoofs TLS fingerprint to look like real Chrome browser

### 4. Run Tool

```bash
python main.py "https://services.sheerid.com/verify/xxx?verificationId=abc123"
```

**With proxy (recommended to avoid fraud detection):**
```bash
python main.py "URL" --proxy 123.45.67.89:8080
python main.py "URL" --proxy http://user:pass@proxy.example.com:8080
```

---

## 🛡️ Avoiding Fraud Detection (`fraudRulesReject`)

If you encounter `fraudRulesReject` error, try:

| Solution | Description |
|----------|-------------|
| **Residential Proxy** | Use `--proxy` flag with residential IP (not datacenter) |
| **Wait Between Attempts** | 5-10 minutes between verifications |
| **Different University** | Tool uses weighted selection for higher success |
| **Fresh Verification Link** | Get a new link if previous one failed |

## ⚙️ How It Works

```
1. Parse verificationId
2. Check link state
3. Generate student identity
4. Generate student ID card
5. Submit → collectStudentPersonalInfo
6. Skip SSO → DELETE /step/sso
7. Upload document → S3
8. Complete → completeDocUpload
```

---

## 🎁 Benefits After Verification

Once verified, you get:

| Benefit | Description |
|---------|-------------|
| **Gemini Advanced** | Most powerful AI model |
| **2TB Google Drive** | Cloud storage |
| **NotebookLM Pro** | AI-powered notes |
| **AI Video Credits** | Veo video generation |

---

## ❌ 常见错误及解决方案

### `invalidOrganization` 错误

**症状**: 提交时返回 400 错误,errorIds 包含 "invalidOrganization"

```json
{
  "errorIds": ["invalidOrganization"],
  "currentStep": "collectStudentPersonalInfo"
}
```

**原因**:
- 所选大学不在 SheerID 数据库中或不被接受
- 地理位置与大学不匹配 (需要美国IP)
- 未使用美国住宅代理
- metadata 字段不完整导致验证失败

**解决方案**:
1. ✅ **确保使用美国住宅代理** - 数据中心IP会被拒绝
2. ✅ **选择权重 >= 95 的大学** - 工具已自动优先选择高成功率学校
3. ✅ **代理位置与大学所在州匹配** - 使用加州代理访问加州大学

**示例**:
```bash
# 使用加州代理访问 UCLA
python main.py "YOUR_URL" --proxy http://user:pass@ca-proxy.example.com:port --force
```

### `fraudRulesReject` 错误

**症状**: 触发反欺诈检测

**原因**:
- 使用数据中心代理而非住宅代理
- 代理IP被标记或在黑名单中
- 请求频率过高
- 浏览器指纹异常

**解决方案**:
1. 使用高质量住宅代理服务
2. 等待 5-10 分钟后重试
3. 确保安装了 `curl_cffi`: `pip install curl_cffi`
4. 尝试不同的大学

---

## 🎯 成功率优化建议

### 1. 使用可靠代理服务

推荐的住宅代理服务:
- **Smartproxy** (smartproxy.com) - 高质量住宅IP
- **Bright Data** (brightdata.com) - 企业级解决方案  
- **Oxylabs** (oxylabs.io) - 稳定可靠

⚠️ **避免使用**:
- 免费代理
- 数据中心代理
- VPN 服务 (大多数会被检测)

### 2. 优先选择高权重大学

工具自动优先选择以下高成功率大学:
- Pennsylvania State University (权重 100)
- University of California, Los Angeles (权重 98)
- University of California, Berkeley (权重 97)
- Massachusetts Institute of Technology (权重 95)
- Stanford University (权重 95)
- University of Michigan (权重 95)

### 3. 确保环境配置正确

```bash
# 安装所有依赖
pip install httpx Pillow curl_cffi

# 验证 curl_cffi 已安装
python -c "import curl_cffi; print('✅ curl_cffi OK')"

# 验证 anti_detect.py 可加载
cd one-verify-tool
python -c "import sys; sys.path.insert(0, '..'); from anti_detect import *; print('✅ anti_detect OK')"
```

### 4. 检查代理配置

```bash
# 测试代理连接
curl --proxy http://user:pass@proxy.example.com:port https://api.ipify.org?format=json

# 应返回美国IP地址
```

---

## 🧠 Intelligent Strategy: University Student

Optimized for Google One (Gemini Advanced) verification:

### 1. Weighted University Selection
-   **Database**: 45+ Universities (Global).
-   **Smart Weighting**: Selects high-success institutions.

### 2. The "Waterfall" Flow
1.  **Submission**: Submits PII.
2.  **SSO Bypass**: Skips school portal login (`DELETE /step/sso`).
3.  **Document Gen**: Creates realistic Student ID cards.
4.  **Completion**: Finalizes upload via `completeDocUpload`.

### 3. Success Factors
-   **Age Targeting**: 18-24 demographic.
-   **Clean Images**: Optimized for OCR.
