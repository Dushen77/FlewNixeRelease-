# FlewNixe 发布版使用说明

本目录包含以下发布文件：
版本 1.5.4
- `FlewNixe_Windows_amd64.exe`
- `FlewNixe_Linux_amd64`
- `FlewNixe_Android_arm64`

## 一、通用说明

程序启动后，仍然需要先通过 FlewNixe 账号体系登录，然后才会进入主菜单或按启动参数直接进入指定功能。

支持两类登录参数：

### 1. 普通账号登录

- `-login-user` / `--login-user`
- `-login-pass` / `--login-pass`

示例：

```bash
./FlewNixe_Linux_amd64 --login-user myuser --login-pass mypass
```

### 2. 经销商 Token 登录

- `-token` / `--token`

示例：

```bash
./FlewNixe_Linux_amd64 --token xxxxxxxxxxxxxxxxxxxxxxxx
```

## 二、[ 3 ] FlewNIxe-Interchanger

### 功能介绍

`[ 3 ] 启动 FlewNIxe-Interchanger（跨服聊天互通）` 用于把两个网易租赁服的聊天栏打通。

主要特性：

- 双向转发：服务器 1 和服务器 2 的玩家聊天互相同步
- 静默桥接：通过 `tellraw` 转发，不让机器人自己在聊天栏里“说话”
- 双服独立配置：两台租赁服可分别设置服号、密码、认证服地址、认证服 Token

### 启动方式

新增直启参数：

- `-start-interchanger`
- `--start-interchanger`

示例：

```bash
./FlewNixe_Linux_amd64 \
  --login-user myuser \
  --login-pass mypass \
  --start-interchanger \
  --i1-rsn 12345678 \
  --i1-rsp 000000 \
  --i2-rsn 87654321 \
  --i2-rsp 000000
```

### 参数说明

#### 1. 服务器 1 参数

- `-i1-rsn` / `--i1-rsn` / `--interchanger-server1-rental-server-code`
  - 服务器 1 的租赁服编号
- `-i1-rsp` / `--i1-rsp` / `--interchanger-server1-rental-server-passcode`
  - 服务器 1 的租赁服密码，默认可为 `000000`
- `-i1-asa` / `--i1-asa` / `--interchanger-server1-auth-server-address`
  - 服务器 1 使用的认证服务器地址
- `-i1-ast` / `--i1-ast` / `--interchanger-server1-auth-server-token`
  - 服务器 1 使用的认证服务器 Token

#### 2. 服务器 2 参数

- `-i2-rsn` / `--i2-rsn` / `--interchanger-server2-rental-server-code`
  - 服务器 2 的租赁服编号
- `-i2-rsp` / `--i2-rsp` / `--interchanger-server2-rental-server-passcode`
  - 服务器 2 的租赁服密码，默认可为 `000000`
- `-i2-asa` / `--i2-asa` / `--interchanger-server2-auth-server-address`
  - 服务器 2 使用的认证服务器地址
- `-i2-ast` / `--i2-ast` / `--interchanger-server2-auth-server-token`
  - 服务器 2 使用的认证服务器 Token

#### 3. 通用认证参数

除分别指定 `i1` / `i2` 的认证参数外，也支持通用认证参数：

- `-asa` / `--asa` / `--auth-server-address`
- `-ast` / `--ast` / `--auth-server-token`

作用规则：

- 若 `i1-asa` / `i1-ast` 未填写，则服务器 1 会回退使用通用 `-asa` / `-ast`
- 若 `i2-asa` / `i2-ast` 未填写，则服务器 2 会回退使用通用 `-asa` / `-ast`
- 若你本地保存过认证服配置，且本次没有通过参数覆盖，则仍会沿用本地已保存配置

### 交互规则

- 带了 `--start-interchanger` 后，程序会自动进入菜单 `[ 3 ]`
- 已通过参数提供的字段，会作为默认值自动使用
- 未提供的字段，程序会继续在终端中询问
- 也就是说，参数可以“全自动直启”，也可以“半自动预填”

### 推荐示例

#### 1. 最简示例

适用于本地已经保存认证服配置，只想传两台服号：

```bash
./FlewNixe_Linux_amd64 \
  --login-user myuser \
  --login-pass mypass \
  --start-interchanger \
  --i1-rsn 12345678 \
  --i2-rsn 87654321
```

#### 2. 完整示例

```bash
./FlewNixe_Linux_amd64 \
  --login-user myuser \
  --login-pass mypass \
  --start-interchanger \
  --i1-rsn 12345678 \
  --i1-rsp 111111 \
  --i1-asa https://auth1.example.com \
  --i1-ast token_server1 \
  --i2-rsn 87654321 \
  --i2-rsp 222222 \
  --i2-asa https://auth2.example.com \
  --i2-ast token_server2
```

## 三、[ 7 ] FlewNixe-CloudChain

### 功能介绍

`[ 7 ] 启动 FlewNixe-CloudChain（QQ 与租赁服信息互通）` 用于把 QQ 群和网易租赁服互通。

主要特性：

- 游戏聊天转发到 QQ 群
- QQ 群消息回传到游戏聊天栏
- 支持系统消息/公告转发开关
- 支持 QQ 链接卡片转发
- 支持管理员在群内使用 `/信息`、`/指令`、`/踢`、`/在线人数`
- 启动时会通过云链密钥自动换取 NapCat HTTP/WS 配置

### 启动方式

新增直启参数：

- `-start-cloudchain`
- `--start-cloudchain`

示例：

```bash
./FlewNixe_Linux_amd64 \
  --login-user myuser \
  --login-pass mypass \
  --start-cloudchain \
  --rsn 12345678 \
  --rsp 000000 \
  --cc-secret your_cloud_secret \
  --cc-group 123456789 \
  --cc-admin 987654321
```

### 参数说明

#### 1. 租赁服参数

CloudChain 复用通用租赁服参数：

- `-rsn` / `--rsn` / `--rental-server-code`
- `-rsp` / `--rsp` / `--rental-server-passcode`
- `-asa` / `--asa` / `--auth-server-address`
- `-ast` / `--ast` / `--auth-server-token`

说明：

- `-rsn` 为 CloudChain 要进入的租赁服编号
- `-rsp` 为租赁服密码，未填时通常可留为 `000000`
- `-asa` / `-ast` 用于覆盖认证服配置

#### 2. 云链与 QQ 参数

- `-cc-secret` / `--cc-secret` / `--cloudchain-secret`
  - 云链密钥，程序会拿它去换取 NapCat HTTP / WebSocket 配置
- `-cc-group` / `--cc-group` / `--cloudchain-group-id`
  - 要转发到的目标 QQ 群号
- `-cc-admin` / `--cc-admin` / `--cloudchain-admin-qq`
  - 群内管理员 QQ 号，允许使用管理命令

#### 3. 行为控制参数

- `-cc-forward-system` / `--cc-forward-system`
  - 是否同时转发系统消息/公告到 QQ
  - 可填：`true/false`、`y/n`、`1/0`
- `-cc-share-card` / `--cc-share-card`
  - 是否把游戏聊天发送为 QQ 链接卡片
- `-cc-share-url` / `--cc-share-url` / `--cloudchain-share-card-url`
  - 当启用卡片转发时，卡片点击跳转链接
- `-cc-share-image` / `--cc-share-image` / `--cloudchain-share-card-image`
  - 卡片缩略图地址，可留空

### 交互规则

- 带了 `--start-cloudchain` 后，程序会自动进入菜单 `[ 7 ]`
- 已通过参数提供的字段，会作为默认值自动使用
- 未提供的字段，程序会继续在终端中询问
- 如果启用了 `--cc-share-card`，但没有提供 `--cc-share-url`，程序仍会继续要求输入卡片链接

### 推荐示例

#### 1. 最简示例

适用于本地已保存认证服配置：

```bash
./FlewNixe_Linux_amd64 \
  --login-user myuser \
  --login-pass mypass \
  --start-cloudchain \
  --rsn 12345678 \
  --cc-secret your_cloud_secret \
  --cc-group 123456789 \
  --cc-admin 987654321
```

#### 2. 开启系统消息转发

```bash
./FlewNixe_Linux_amd64 \
  --login-user myuser \
  --login-pass mypass \
  --start-cloudchain \
  --rsn 12345678 \
  --rsp 000000 \
  --cc-secret your_cloud_secret \
  --cc-group 123456789 \
  --cc-admin 987654321 \
  --cc-forward-system true
```

#### 3. 开启 QQ 卡片转发

```bash
./FlewNixe_Linux_amd64 \
  --login-user myuser \
  --login-pass mypass \
  --start-cloudchain \
  --rsn 12345678 \
  --cc-secret your_cloud_secret \
  --cc-group 123456789 \
  --cc-admin 987654321 \
  --cc-share-card true \
  --cc-share-url https://example.com/server-home \
  --cc-share-image https://example.com/cover.png
```

## 四、参数互斥规则

以下直启功能一次只能选一个：

- `--start-builder`
- `--start-interchanger`
- `--start-cloudchain`

如果同时传了多个，程序会直接报错并退出。

## 五、Windows 启动示例

### Interchanger

```bash
.\FlewNixe_Windows_amd64.exe --login-user myuser --login-pass mypass --start-interchanger --i1-rsn 12345678 --i2-rsn 87654321
```

### CloudChain

```bash
.\FlewNixe_Windows_amd64.exe --login-user myuser --login-pass mypass --start-cloudchain --rsn 12345678 --cc-secret your_cloud_secret --cc-group 123456789 --cc-admin 987654321
```

## 六、补充说明

1. 所有布尔参数都支持以下写法：
   - `true / false`
   - `1 / 0`
   - `y / n`
   - `yes / no`
   - `on / off`

2. 若只写参数名、不写值：
   - 例如 `--start-cloudchain`
   - 会按 `true` 处理

3. 若你已经保存过认证服配置：
   - 普通情况下会直接复用
   - 传入 `-asa` / `-ast` 后会优先使用本次命令行给定值

4. 本次新增的启动参数，核心目标是：
   - 允许直接启动 `[ 3 ]` 和 `[ 7 ]`
   - 允许在发版后通过一条命令快速拉起
   - 允许在参数不完整时保留原本的交互式补录流程
