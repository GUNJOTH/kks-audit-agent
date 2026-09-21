# 发布约定

本文档规定 `kks-audit-agent` 的版本、检查和 GitHub Release 流程。当前仓库没有自动
发布工作流，因此发布由仓库维护者按本约定手动完成。

## 版本与变更记录

- 版本号以 `pyproject.toml` 的 `[project].version` 为准。
- 使用 `MAJOR.MINOR.PATCH` 版本格式，并使用 `vMAJOR.MINOR.PATCH` 作为 Git tag。
- 用户可见的功能、修复、兼容性变化和审核规则变化必须先在 `CHANGELOG.md` 的对应
  版本条目中记录。
- 版本号和更新日志在同一个 Pull Request 中修改；PR 目标为 `main`，不得直接推送
  或强推 `main`。
- 尚未形成正式版本的变化写入 `[未发布]`，不要在没有实际发布内容时伪造历史版本
  条目。

## 发布前检查

合并版本 PR 前，至少执行以下锁定依赖、编译和测试检查：

```powershell
uv sync --locked
uv run python -m compileall -q .
uv run python -m unittest discover -s tests -p "test_*.py"
```

如果本次发布包含 Windows EXE，还要在受控 Windows 环境执行：

```powershell
powershell -ExecutionPolicy Bypass -File .\build_exe.ps1 -Clean
```

并确认 `dist\KKS-Audit-Agent\KKS-Audit-Agent.exe` 及其运行所需文件完整。发布包、
审核报告、日志和 Release 说明中不得包含真实 API 密钥、Token、真实业务 Excel 或
其他敏感信息。

如果默认分支的 CI 尚未覆盖本次变更，必须在 PR 中附上上述命令的实际输出；CI 合并
到默认分支后，以对应工作流结果作为持续验证证据。

## 发布步骤

1. 从 `main` 创建版本 PR，更新 `pyproject.toml` 版本号和 `CHANGELOG.md`。
2. 等待 PR 检查通过，并完成代码、审核规则、打包边界和敏感信息审查后合并。
3. 在已合并的 `main` 提交上创建对应的 `vMAJOR.MINOR.PATCH` tag，并推送该 tag；
   不修改或覆盖已有 tag。
4. 基于该 tag 创建 GitHub Release，标题使用版本号，正文引用
   `CHANGELOG.md` 中对应版本条目，并注明是否包含 Windows EXE、规则变化和已知限制。
5. 发布后确认 Release、tag 和 `pyproject.toml` 版本一致；如发现问题，按补丁版本
   发布修复，不回写已发布版本。

## 回滚边界

发布回滚不得通过删除或覆盖 tag 伪造历史。应用代码、审核规则、用户配置和 EXE 产物
的回滚必须分别评估；如果规则或产物已经被使用，应保留原版本证据，并通过新版本发布
修复或明确的迁移说明处理。
