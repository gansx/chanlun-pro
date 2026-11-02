# chanlun-pro fork 自动同步说明

Fork 自 [yijixiuxin/chanlun-pro](https://github.com/yijixiuxin/chanlun-pro)。

## 同步配置

| 项 | 值 |
|---|---|
| 上游 | `yijixiuxin/chanlun-pro` (master) |
| 本仓库 | `gansx/chanlun-pro` (master) |
| 频率 | 每周一 **02:30 UTC** = 北京时间 **10:30** |
| 方式 | `git merge upstream/master` |
| 冲突 | 自动回滚 + 开 issue（`upstream-sync`） |
| 超时 | 30 分钟（仓库 432 MB） |

## 这个仓库的特殊性

**95 MB 是加密的。** 同步只能拿到：

| 同步得到的 | 状态 |
|---|---|
| 数据源适配器（`exchange/`，23 文件） | ✅ 明文 |
| 回测引擎（`backtesting/`，9 文件） | ✅ 明文 |
| 17 个策略（`strategy/`） | ✅ 明文 |
| 接口契约（`cl_interface.py`） | ✅ 明文 |
| 背驰/多级别算法（`cl_utils.py`/`cl_analyse.py`） | ✅ 明文 |
| **核心算法（`core/`，8 文件）** | ❌ **Pyarmor 密文** |

**同步不会让核心变明文。** 那层永远是密文，需要 `pyarmor.rkey`。

## 上游没有任何 workflow

这个仓库不带 CI，所以同步时不会有 workflow 冲突，也不需要禁用任何东西。

## 手动同步

```bash
# GitHub CLI
gh workflow run sync-upstream.yml --repo gansx/chanlun-pro

# 本地
git fetch upstream && git merge upstream/master && git push
```

## 仓库体积提醒

432 MB。clone 建议：

```bash
# 只要最新代码
git clone --depth 1 https://github.com/gansx/chanlun-pro.git

# 需要完整历史（同步、cherry-pick）
git clone https://github.com/gansx/chanlun-pro.git
```

## 关于公开/私有

当前是 **public**：
- ✅ Actions 完全免费、不计量
- ✅ fork 关系正常，自动同步可用

如果以后转 private：
- ⚠️ fork 关系会断开
- ⚠️ Actions 开始计费（Free 账号 2000 分钟/月）
- ⚠️ `GITHUB_TOKEN` 可能没写权限，需要配 PAT（secret `SYNC_TOKEN`）
- 同步 workflow 要改成用 PAT：

```yaml
with:
  token: ${{ secrets.SYNC_TOKEN || secrets.GITHUB_TOKEN }}
```

## 要拿核心算法怎么办

唯一路径：20 天免费试用 rkey。

见仓库根目录的 `cookbook/docs/pricing-and-contact.md`：
- 加作者微信申请（`cookbook/docs/img/wx.jpg`）
- **先装好环境**（能跑到 `missing license key` 再申请）
- 拿到后放 `src/pyarmor_runtime_005445/pyarmor.rkey`
