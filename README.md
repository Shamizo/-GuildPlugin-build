# 工会插件 (Guild) 原作者:chenasyd 本分支仅构建了一下无任何修改！！！！

一个功能完整的Minecraft工会系统插件，支持工会创建、成员管理、经济系统、关系管理等功能。

## 📋 目录

- [功能特性](#功能特性)
- [安装指南](#安装指南)
- [配置说明](#配置说明)
- [命令列表](#命令列表)
- [权限节点](#权限节点)
- [GUI界面](#gui界面)
- [经济系统](#经济系统)
- [工会关系](#工会关系)
- [等级系统](#等级系统)
- [数据库](#数据库)
- [常见问题](#常见问题)
- [更新日志](#更新日志)

## ✨ 功能特性

### 核心功能
- **工会创建与管理**: 支持工会创建、删除、信息查看
- **成员管理**: 邀请、踢出、提升、降级成员
- **权限系统**: 基于角色的权限管理（会长、官员、成员）
- **GUI界面**: 完整的图形用户界面，操作便捷
- **经济系统**: 工会资金管理、存款、取款、转账
- **等级系统**: 工会等级提升，增加成员上限
- **关系管理**: 工会间关系（盟友、敌对、中立、开战、停战）
- **工会家**: 设置和传送到工会家
- **申请系统**: 玩家申请加入工会

### 高级功能
- **异步处理**: 所有数据库操作均为异步，不影响服务器性能
- **多数据库支持**: 支持SQLite和MySQL
- **占位符支持**: 集成PlaceholderAPI
- **经济集成**: 通过Vault支持多种经济插件
- **权限集成**: 与Bukkit权限系统完全集成

## 🚀 安装指南

### 前置要求
- **Minecraft服务器**: 1.13+ (推荐1.21+)
- **Java**: JDK 8+ (推荐JDK 17+)
- **Vault**: 经济系统支持 (可选)
- **PlaceholderAPI**: 占位符支持 (可选)

### 安装步骤

1. **下载插件**
   ```bash
   # 从发布页面下载最新版本的jar文件
   # 或使用Maven编译
   mvn clean package
   ```

2. **安装到服务器**
   ```bash
   # 将编译好的jar文件放入plugins文件夹
   cp target/guild-plugin-1.0.0.jar plugins/
   ```

3. **启动服务器**
   ```bash
   # 启动服务器，插件会自动生成配置文件
   java -jar server.jar
   ```

4. **配置插件**
   ```bash
   # 编辑生成的配置文件
   nano plugins/GuildPlugin/config.yml
   nano plugins/GuildPlugin/messages.yml
   nano plugins/GuildPlugin/gui.yml
   nano plugins/GuildPlugin/database.yml
   ```

5. **重启服务器**
   ```bash
   # 重启服务器使配置生效
   restart
   ```

## ⚙️ 配置说明

### 主配置文件 (config.yml)

```yaml
# 数据库配置
database:
  type: sqlite  # sqlite 或 mysql
  mysql:
    host: localhost
    port: 3306
    database: guild
    username: root
    password: ""
    pool-size: 10

# 工会配置
guild:
  min-name-length: 3
  max-name-length: 20
  max-tag-length: 6
  max-description-length: 100
  max-members: 50
  creation-cost: 1000.0  # 创建工会费用

# 权限配置
permissions:
  default:
    can-create: true
    can-invite: true
    can-kick: true
    can-promote: true
    can-demote: false
    can-delete: false
```

### 消息配置文件 (messages.yml)

```yaml
# 通用消息
general:
  prefix: "&6[工会] &r"
  no-permission: "&c您没有权限执行此操作！"

# 工会创建消息
create:
  success: "&a工会 {name} 创建成功！"
  insufficient-funds: "&c您的余额不足！创建工会需要 {cost} 金币。"
```

### GUI配置文件 (gui.yml)

```yaml
# 主界面配置
main-menu:
  title: "&6工会系统"
  size: 54
  items:
    create-guild:
      slot: 4
      material: EMERALD_BLOCK
      name: "&a创建工会"
      lore:
        - "&7创建新的工会"
        - "&7需要消耗金币"
```

### 数据库配置文件 (database.yml)

```yaml
# SQLite配置
sqlite:
  file: "plugins/GuildPlugin/guild.db"
  
# MySQL配置
mysql:
  host: localhost
  port: 3306
  database: guild
  username: root
  password: ""
  pool-size: 10
  min-idle: 5
  connection-timeout: 30000
  idle-timeout: 600000
  max-lifetime: 1800000
```

## 📝 命令列表

### 玩家命令

| 命令 | 权限 | 描述 |
|------|------|------|
| `/guild` | `guild.use` | 打开工会主界面 |
| `/guild create <名称> [标签] [描述]` | `guild.create` | 创建工会 |
| `/guild info` | `guild.info` | 查看工会信息 |
| `/guild members` | `guild.members` | 查看工会成员 |
| `/guild invite <玩家>` | `guild.invite` | 邀请玩家加入工会 |
| `/guild kick <玩家>` | `guild.kick` | 踢出工会成员 |
| `/guild leave` | `guild.leave` | 离开工会 |
| `/guild delete` | `guild.delete` | 删除工会 |
| `/guild promote <玩家>` | `guild.promote` | 提升成员职位 |
| `/guild demote <玩家>` | `guild.demote` | 降级成员职位 |
| `/guild accept <邀请者>` | `guild.accept` | 接受工会邀请 |
| `/guild decline <邀请者>` | `guild.decline` | 拒绝工会邀请 |
| `/guild sethome` | `guild.sethome` | 设置工会家 |
| `/guild home` | `guild.home` | 传送到工会家 |
| `/guild apply <工会> [消息]` | `guild.apply` | 申请加入工会 |

### 管理员命令

| 命令 | 权限 | 描述 |
|------|------|------|
| `/guildadmin` | `guild.admin` | 管理员主命令 |
| `/guildadmin reload` | `guild.admin.reload` | 重载配置文件 |
| `/guildadmin list` | `guild.admin.list` | 列出所有工会 |
| `/guildadmin info <工会>` | `guild.admin.info` | 查看工会详细信息 |
| `/guildadmin delete <工会>` | `guild.admin.delete` | 强制删除工会 |
| `/guildadmin kick <玩家> <工会>` | `guild.admin.kick` | 从工会踢出玩家 |
| `/guildadmin relation` | `guild.admin.relation` | 关系管理 |
| `/guildadmin test` | `guild.admin.test` | 测试功能 |

## 🔐 权限节点

### 基础权限
- `guild.use` - 使用工会系统
- `guild.create` - 创建工会
- `guild.info` - 查看工会信息
- `guild.members` - 查看工会成员
- `guild.invite` - 邀请玩家
- `guild.kick` - 踢出成员
- `guild.leave` - 离开工会
- `guild.delete` - 删除工会
- `guild.promote` - 提升成员
- `guild.demote` - 降级成员
- `guild.accept` - 接受邀请
- `guild.decline` - 拒绝邀请
- `guild.sethome` - 设置工会家
- `guild.home` - 传送到工会家
- `guild.apply` - 申请加入工会

### 管理员权限
- `guild.admin` - 管理员权限
- `guild.admin.reload` - 重载配置
- `guild.admin.list` - 查看所有工会
- `guild.admin.info` - 查看工会详情
- `guild.admin.delete` - 强制删除工会
- `guild.admin.kick` - 强制踢出玩家
- `guild.admin.relation` - 管理工会关系
- `guild.admin.test` - 测试功能

## 🖥️ GUI界面

### 主界面
- **创建工会**: 创建新的工会
- **工会信息**: 查看当前工会信息
- **成员管理**: 管理工会成员
- **申请管理**: 处理加入申请
- **工会设置**: 修改工会设置
- **工会列表**: 查看所有工会
- **工会关系**: 管理工会关系

### 创建工会界面
- **工会名称输入**: 设置工会名称（3-20字符）
- **工会标签输入**: 设置工会标签（最多6字符，可选）
- **工会描述输入**: 设置工会描述（最多100字符，可选）
- **确认创建**: 支付费用创建工会
- **取消**: 返回主界面

### 成员管理界面
- **成员列表**: 显示所有成员
- **邀请成员**: 邀请新成员
- **踢出成员**: 踢出现有成员
- **提升成员**: 提升成员职位
- **降级成员**: 降级成员职位

## 💰 经济系统

### 功能特性
- **工会资金**: 每个工会独立的资金账户
- **存款系统**: 成员可以向工会存款
- **取款系统**: 成员可以从工会取款
- **转账系统**: 工会间资金转账
- **贡献记录**: 记录每个成员的贡献
- **等级升级**: 资金达到要求自动升级

### 等级系统

| 等级 | 资金要求 | 最大成员数 |
|------|----------|------------|
| 1 | 0-5,000 | 6 |
| 2 | 5,000-10,000 | 12 |
| 3 | 10,000-20,000 | 18 |
| 4 | 20,000-35,000 | 24 |
| 5 | 35,000-50,000 | 30 |
| 6 | 50,000-75,000 | 40 |
| 7 | 75,000-100,000 | 50 |
| 8 | 100,000-150,000 | 60 |
| 9 | 150,000-200,000 | 80 |
| 10 | 200,000+ | 100 |

### 经济命令
- `/guild deposit <金额>` - 向工会存款
- `/guild withdraw <金额>` - 从工会取款
- `/guild transfer <工会> <金额>` - 向其他工会转账
- `/guild balance` - 查看工会余额

## 🤝 工会关系

### 关系类型
- **中立 (Neutral)**: 默认关系，无特殊效果
- **盟友 (Ally)**: 友好关系，显示为绿色
- **敌对 (Enemy)**: 敌对关系，显示为红色
- **开战 (War)**: 战争状态，登录时通知
- **停战 (Truce)**: 临时停战，需要双方同意结束

### 关系管理
- **创建关系**: 工会会长可以创建关系
- **接受关系**: 目标工会需要接受关系
- **拒绝关系**: 目标工会可以拒绝关系
- **取消关系**: 可以取消已建立的关系
- **关系过期**: 关系有自动过期机制

### 关系命令
- `/guild relation create <工会> <类型>` - 创建关系
- `/guild relation accept <工会>` - 接受关系
- `/guild relation reject <工会>` - 拒绝关系
- `/guild relation cancel <工会>` - 取消关系

## 🗄️ 数据库

### 数据表结构

#### guilds (工会表)
```sql
CREATE TABLE guilds (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name VARCHAR(20) UNIQUE NOT NULL,
    tag VARCHAR(6),
    description TEXT,
    leader_uuid VARCHAR(36) NOT NULL,
    leader_name VARCHAR(16) NOT NULL,
    balance DOUBLE DEFAULT 0.0,
    level INTEGER DEFAULT 1,
    max_members INTEGER DEFAULT 6,
    home_world VARCHAR(64),
    home_x DOUBLE,
    home_y DOUBLE,
    home_z DOUBLE,
    home_yaw FLOAT,
    home_pitch FLOAT,
    frozen BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### guild_members (成员表)
```sql
CREATE TABLE guild_members (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    guild_id INTEGER NOT NULL,
    player_uuid VARCHAR(36) NOT NULL,
    player_name VARCHAR(16) NOT NULL,
    role VARCHAR(20) DEFAULT 'MEMBER',
    joined_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (guild_id) REFERENCES guilds(id) ON DELETE CASCADE
);
```

#### guild_applications (申请表)
```sql
CREATE TABLE guild_applications (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    guild_id INTEGER NOT NULL,
    player_uuid VARCHAR(36) NOT NULL,
    player_name VARCHAR(16) NOT NULL,
    message TEXT,
    status VARCHAR(20) DEFAULT 'PENDING',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (guild_id) REFERENCES guilds(id) ON DELETE CASCADE
);
```

#### guild_relations (关系表)
```sql
CREATE TABLE guild_relations (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    guild1_id INTEGER NOT NULL,
    guild2_id INTEGER NOT NULL,
    relation_type VARCHAR(20) NOT NULL,
    status VARCHAR(20) DEFAULT 'PENDING',
    initiator_uuid VARCHAR(36) NOT NULL,
    initiator_name VARCHAR(16) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    expires_at TIMESTAMP,
    FOREIGN KEY (guild1_id) REFERENCES guilds(id) ON DELETE CASCADE,
    FOREIGN KEY (guild2_id) REFERENCES guilds(id) ON DELETE CASCADE
);
```

#### guild_economy (经济表)
```sql
CREATE TABLE guild_economy (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    guild_id INTEGER NOT NULL,
    player_uuid VARCHAR(36) NOT NULL,
    player_name VARCHAR(16) NOT NULL,
    amount DOUBLE NOT NULL,
    type VARCHAR(20) NOT NULL,
    description TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (guild_id) REFERENCES guilds(id) ON DELETE CASCADE
);
```

## ❓ 常见问题

### Q: 插件无法启动怎么办？
A: 检查以下几点：
1. 确保服务器版本兼容（1.13+）
2. 检查Java版本（JDK 8+）
3. 查看控制台错误信息
4. 确保配置文件格式正确

### Q: 经济系统不工作？
A: 确保：
1. 安装了Vault插件
2. 安装了经济插件（如EssentialsX）
3. 在config.yml中正确配置了经济系统

### Q: 数据库连接失败？
A: 检查：
1. 数据库配置是否正确
2. MySQL服务器是否运行
3. 数据库用户权限是否足够
4. 网络连接是否正常

### Q: GUI界面显示异常？
A: 可能原因：
1. 配置文件格式错误
2. 颜色代码格式不正确
3. 变量替换失败
4. 权限不足

### Q: 工会创建失败？
A: 检查：
1. 玩家是否有足够金币
2. 工会名称是否已存在
3. 玩家是否已在其他工会
4. 名称长度是否符合要求

## 📝 更新日志

### v1.0.0 (当前版本)
- ✨ 初始版本发布
- 🎯 完整的工会管理系统
- 💰 经济系统集成
- 🤝 工会关系管理
- 📊 等级系统
- 🖥️ 完整的GUI界面
- 🗄️ 多数据库支持
- 🔐 权限系统
- 📱 占位符支持

### 计划功能
- [ ] 工会战争系统(部分实现)
- [ ] 工会商店
- [ ] 工会任务系统
- [ ] 工会排行榜
- [ ] 工会活动系统
- [ ] 工会仓库
- [ ] 工会公告系统
- [ ] 工会日志系统

