# 贡献指南

## 开始前

先阅读 README、GOVERNANCE.md 和 SECURITY.md。使用 Python 3.11+、uv 和项目已有依赖，不擅自替换锁文件或运行工具链。

## 分支与 Pull Request

从 main 创建用途明确的开发分支，例如 feat/<short-name>、fix/<short-name>、docs/<short-name> 或 chore/<short-name>。不要直接向 main 推送。

Pull Request 应说明变更目的、规则或 API 影响、实际验证命令、风险、兼容性和回滚方式。涉及审核规则时，必须说明规则来源和 Excel 证据边界。

## 提交前检查

~~~powershell
uv sync --locked
uv run python -m compileall -q .
uv run python -m unittest discover -s tests -p "test_*.py"
~~~

真实 AI、数据库和生产文件验证必须单独记录环境和授权范围。不得为了让 CI 通过而删除测试、降低断言或提交敏感样本。