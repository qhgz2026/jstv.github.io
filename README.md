# 2026届初中同学录 - 版本B（安全版）部署指南

## 📦 文件清单

```
同学录系统/
├── index.html              ← 主页面（直接打开或部署到GitHub Pages）
├── data/
│   ├── classes.json        ← 班级列表配置
│   ├── class-1/
│   │   ├── students.json   ← 高三(1)班同学数据
│   │   └── roles.json      ← 高三(1)班职务配置
│   ├── class-2/
│   │   ├── students.json   ← 高三(2)班同学数据
│   │   └── roles.json      ← 高三(2)班职务配置
│   └── class-3/
│       ├── students.json   ← 高三(3)班同学数据
│       └── roles.json      ← 高三(3)班职务配置
```

---

## 🚀 快速部署（5分钟搞定）

### 第一步：创建GitHub仓库

1. 登录 [github.com](https://github.com)
2. 点击右上角 **+** → **New repository**
3. 仓库名填写：`class2026-alumni`（或你喜欢的名字）
4. 选择 **Public**（必须公开，否则GitHub Raw无法访问）
5. 勾选 **Add a README file**
6. 点击 **Create repository**

### 第二步：上传文件

1. 在仓库页面点击 **Add file** → **Upload files**
2. 将 `index.html` 拖到上传区域
3. 创建文件夹 `data/`，上传 `classes.json`
4. 创建文件夹 `data/class-1/`，上传 `students.json` 和 `roles.json`
5. 创建文件夹 `data/class-2/`，上传 `students.json` 和 `roles.json`
6. 创建文件夹 `data/class-3/`，上传 `students.json` 和 `roles.json`
7. 点击 **Commit changes**

### 第三步：开启GitHub Pages

1. 仓库页面点击 **Settings** → 左侧 **Pages**
2. **Source** 选择 **Deploy from a branch**
3. **Branch** 选择 **main** → **/(root)**
4. 点击 **Save**
5. 等待1-2分钟，页面会显示访问链接：`https://你的用户名.github.io/class2026-alumni/`

### 第四步：修改配置

1. 打开 `index.html`
2. 找到第3行的配置区：
```javascript
const CONFIG = {
    GITHUB_OWNER: '你的GitHub用户名',      // ← 改成你的GitHub用户名
    GITHUB_REPO: 'class2026-alumni',      // ← 改成你的仓库名
    ...
};
```
3. 修改后重新上传 `index.html`

---

## 🔑 如何获取GitHub Token

Token用于班长/管理员提交数据修改（创建Pull Request）。

1. 登录GitHub → 点击右上角头像 → **Settings**
2. 左侧最下方 → **Developer settings** → **Personal access tokens** → **Tokens (classic)**
3. 点击 **Generate new token (classic)**
4. 填写：
   - **Note**: 同学录管理
   - **Expiration**: No expiration（或选择有效期）
   - **Scopes**: 勾选 **repo**（完整仓库权限）
5. 点击 **Generate token**
6. **立即复制保存**（⚠️ 只显示一次！）

**安全提醒**：Token相当于密码，请勿泄露给他人。建议仅班长和超级管理员持有。

---

## 👥 权限体系

| 身份 | 权限 |
|------|------|
| **超级管理员** | 管理所有班级、任命任何职务、查看所有号码 |
| **班长** | 管理本班、任命本班班委、查看本班号码 |
| **副班长** | 管理本班、查看本班号码 |
| **团委/学委/体委** | 查看本班号码 |
| **普通同学/访客** | 仅查看公开信息，号码隐藏 |

---

## 🏅 职务任命流程

1. 班长登录后点击 **职务管理**
2. 选择班级 → 选择职务类型（班长/副班长/团委/学委/体委等）
3. 输入同学姓名 → 点击 **提交任命**
4. 系统自动创建Pull Request
5. 超级管理员（你）在GitHub上审核合并
6. 合并后刷新页面，该同学卡片上显示职务标签

---

## ➕ 添加同学流程

1. 班长/副班长登录后点击 **添加同学**
2. 填写信息（姓名、性别、班级必填，其他可选）
3. 点击 **提交审核**
4. 系统自动创建Pull Request
5. 审核通过后数据生效

---

## 📱 电话号码隐私规则

- **访客/非同班同学**：显示 `138****5678`
- **同班同学/班委**：点击"显示完整号码"可查看
- **自己**：始终可见完整号码

---

## 🔄 数据更新

GitHub Pages有缓存，数据修改后可能需要：
1. 强制刷新页面：`Ctrl + F5`（Windows）或 `Cmd + Shift + R`（Mac）
2. 或在URL后加随机参数：`?t=123456`

---

## 🛠️ 高级配置

### 添加新班级

1. 在 `data/classes.json` 中添加：
```json
{"id": "class-4", "name": "高三(4)班"}
```
2. 创建文件夹 `data/class-4/`
3. 上传 `students.json`（空数组 `[]`）和 `roles.json`（空对象 `{}`）

### 添加新职务

在 `index.html` 的 `CONFIG.ROLES` 中添加：
```javascript
artCommittee: {name:'文艺委员', level:3, canManageClass:false, canAssign:false}
```

---

## ❓ 常见问题

**Q: 为什么提交后页面没有变化？**
A: Pull Request需要审核合并后才能生效。请检查GitHub仓库的Pull Requests页面。

**Q: 如何删除同学？**
A: 直接在GitHub上编辑对应的 `students.json` 文件，删除对应条目后提交。

**Q: 可以部署到其他平台吗？**
A: 可以。只要支持静态页面托管即可（Vercel、Netlify、腾讯云COS等）。但GitHub API调用功能需要仓库是GitHub上的。

**Q: 数据安全吗？**
A: 电话号码对非同班同学隐藏。GitHub仓库是公开的，但敏感信息已做前端隐藏处理。如需更高安全性，建议将仓库设为Private并使用GitHub Enterprise。

---

## 📞 技术支持

如有问题，请检查：
1. 浏览器控制台（F12）是否有报错
2. GitHub Token是否有 `repo` 权限
3. 仓库名和用户名是否填写正确
4. 文件路径是否正确（区分大小写）

---

**部署完成！** 🎉 将链接发给同学们即可使用。
