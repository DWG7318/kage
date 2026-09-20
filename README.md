# 臻晨健康 Kage 实例

本仓库使用 Git subtree 保存 Kage 上游代码。

- 实例目录：`instances/zenithcare`
- 上游仓库：`https://github.com/DWG7318/kage.git`
- 上游分支：`main`
- 初始上游提交：`13de68f788962d15464f208e2fdbd00f5b144cc1`
- 改版范围：替换图片与文字，必要时隐藏旧内容

执行计划：

- [文案计划](docs/COPY_PLAN.md)
- [视觉素材计划](docs/VISUAL_ASSET_PLAN.md)

同步上游：

```powershell
git subtree pull --prefix=instances/zenithcare kage-upstream main --squash
```

实例修改只在 `instances/zenithcare` 内进行。原 Kage 历史由 subtree 合并记录保留。
