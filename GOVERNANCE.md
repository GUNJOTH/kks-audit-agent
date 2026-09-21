# 项目治理约定

## 项目范围和事实来源

本仓库维护 KKS Excel 编码审核服务。项目事实以源码、pyproject.toml、uv.lock、审核规则、测试、CI 结果和 Pull Request 记录为准。审核规则与 Excel 证据必须分开管理，AI 只作为辅助复核，不替代规则结论。

## 分支与合并

main 是默认交付分支。变更通过 Pull Request 合并，禁止直接向 main 推送。开发分支使用 feat/、fix/、docs/、chore/ 等用途前缀。当前 Ruleset 禁止删除和非 fast-forward 更新；人工审批数量仍是单独的治理决策。

## CI 与验证边界

CI 执行锁定依赖安装、Python 编译检查和 unittest；不连接真实 AI 服务、数据库或生产 Excel。CI 通过不代表真实 Excel、AI、DM8/LOCATIONS 或部署环境验收通过。

## 数据与安全

Excel、审核报告、AI 配置、管理令牌和运行产物都可能包含业务敏感信息。不得提交真实生产 Excel、API Key、管理令牌、数据库凭据、未脱敏日志或内部地址。管理接口应通过 KKS_ADMIN_TOKEN 和受控网络暴露。