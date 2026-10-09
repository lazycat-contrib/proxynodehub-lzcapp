# proxynodehub-lzcapp

[ProxyNodeHub](https://github.com/wanvfx/ProxyNodeHub) 的懒猫微服打包。

用于发现 GitHub 上活跃的免费节点仓库：分析活跃度、去重分散的节点、直接给出可用的订阅链接。
点一次「搜索并分析」，剩下的梳理工作全部由它完成（Docker Web 工作台版）。

## 交付方式（镜像模式）

引用上游 CI 构建并做过 HTTP 冒烟测试的 Docker Hub 镜像
（`helloworldz1024/proxynodehub`），经 `docker.1ms.run` 加速交付（digest 校验）：

- [`lzc-manifest.yml`](lzc-manifest.yml) — 主服务 `proxynodehub:8080`，数据持久化到
  `/lzcapp/var/data`；登录页由 `injects` 自动填充部署参数中的随机密码
- [`lzc-deploy-params.yml`](lzc-deploy-params.yml) — 安装参数：管理员密码（默认随机 24 位）、
  可选 GitHub Token
- [`lazycat.yml`](.github/workflows/lazycat.yml) / [`lazycat-action.yml`](.github/lazycat-action.yml)
  — [lazycat-github-action](https://github.com/ca-x/lazycat-github-action) 按镜像 digest
  自动发版：`latest` 有更新时 patch 递增版本并上架喵喵（私有）商店；无变化则幂等跳过

## 安装与使用

- 安装时自动生成随机管理员密码（可在安装界面自定义，至少 12 位）；登录页会自动填充，
  也可以到懒猫「应用设置」里查看/修改部署参数
- 可选填 GitHub Token（提高仓库搜索配额）；也可以安装后在应用内「服务设置」配置
- 数据（设置、收藏、任务记录、密钥环）保存在应用持久目录，升级不丢失

## 更新流程

- 上游推送新镜像后，每日定时任务检测到 `latest` digest 变化 → 自动 patch 版本
  （0.1.0 → 0.1.1 → …）→ 发布 Release 资产并上架商店
- 也可以手动触发 `lazycat.yml` 立即检查

## 版权

内容来自 [wanvfx/ProxyNodeHub](https://github.com/wanvfx/ProxyNodeHub)（MIT）；
本仓库仅为打包，不修改上游内容。
