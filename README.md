# Borgmatic Template

这是一个演示项目，目的是使用 Borgmatic 结合 Docker 来备份数据。

---

## 项目结构

```
borgmatic-template/
├── data/
│   ├── borg-repository/         # Borg 备份仓库目录
│   └── borgmatic.d/            # Borgmatic 配置文件目录
│       ├── book.yaml           # 示例备份配置
│       ├── music.yaml          # 示例备份配置
│       ├── user.yaml           # 示例备份配置
│       └── includes/
│           └── common.yaml     # 公共配置片段
├── docker-compose.yaml         # Docker Compose 配置
├── README.md                   # 项目说明文档
```

## 快速开始

> **绿联 NAS（如 DX4800）用户说明：**
> - 绿联 NAS 自带 Docker，无需单独安装。
> - 每次新增一个备份任务（即新增一个配置文件），都需要在容器内执行一次 `borgmatic init --encryption repokey`，以初始化对应的仓库。

1. **准备环境**
   - 需要安装 [Docker](https://docs.docker.com/get-docker/) 和 [Docker Compose](https://docs.docker.com/compose/)。
   - 绿联 NAS 用户可跳过 Docker 安装，直接使用自带 Docker。

2. **启动 Borgmatic 容器**
   ```bash
   docker-compose up -d
   ```

3. **进入容器**
   ```bash
   docker exec -it borgmatic /bin/sh
   ```

4. **初始化备份仓库**
   ```bash
   borgmatic init --encryption repokey
   ```
   > **注意：** 每次新增一个备份任务（新增配置文件）时，都需要为该任务单独执行一次 `borgmatic init --encryption repokey`。

5. **执行备份**
   ```bash
   borgmatic --stats -v 1 --files
   ```

## 重要提示

- **docker-compose.yaml 配置需根据自身需求修改，尤其是环境变量部分。**
- **BORG_PASSPHRASE 务必更换为你自己的安全密码，切勿使用默认或公开密码。**
- 如果你使用 Git 管理本项目，强烈建议将仓库设置为私有，以保护敏感信息（如密码）。
- 如果必须公开仓库，建议引入 [sops](https://github.com/getsops/sops) 等加密工具，对敏感配置文件进行加密管理。

## 常用命令

- 进入容器：
  ```bash
  docker exec -it borgmatic /bin/sh
  ```
- 初始化仓库：
  ```bash
  borgmatic init --encryption repokey
  ```
- 执行备份：
  ```bash
  borgmatic --stats -v 1 --files
  ```

## 配置说明

- `data/borgmatic.d/*.yaml`：每个 YAML 文件为一个备份任务配置。
- `data/borgmatic.d/includes/common.yaml`：可被多个任务 include 的公共配置片段。
- 你可以根据实际需求新增或修改 YAML 配置文件。

## 备份与恢复流程

1. **备份**：
   - 按需编辑 `data/borgmatic.d/*.yaml` 配置文件。
   - 进入容器后执行 `borgmatic` 命令进行备份。
2. **恢复**：
   TODO

## 参考链接

- [Docker Borgmatic](https://github.com/borgmatic-collective/docker-borgmatic)
- [Borgmatic 官方文档](https://torsion.org/borgmatic/)
- [BorgBackup 官方文档](https://borgbackup.readthedocs.io/)
- [Docker 官方文档](https://docs.docker.com/)

---

如有问题欢迎提 issue 或 PR。
