# 炔烃四类通关游戏 V3.6

V3.6 在 V3.5 的 13 个正式任务、100 分计分、关尾必修错题变式、20 列 CSV 和 ALK 学习短码基础上，接入教师提供的 QuickForm 数据任务：

`https://quickform.cn/api/d36lmyvekx`

## 文件

- `炔烃四类通关游戏_V3.6.html`：学生端，单文件，可离线答题。
- `炔烃四类通关游戏_V3.6_教师解析器.html`：教师端，离线导入 QuickForm JSON、ALK34—ALK36 短码和 20 列 CSV。
- `炔烃四类通关游戏_V3.6_GitHub_Pages发布包.zip`：可直接部署的完整包，`index.html` 是 V3.6。

## 学生端收集规则

1. 答题、计分和本地存档不依赖网络。
2. 每关正式题和必修变式全部完成后，才出现该关 QuickForm 提交区。
3. 学生必须先勾选数据说明，再主动点击提交；页面不会自动上报。
4. 每名学生最多形成 4 个阶段检查点：`S1`、`S2`、`S3`、`S4`。后一个检查点包含此前全部累计进度，所以学生只交最终 `S4` 也能形成完整记录。
5. 每条记录使用稳定提交键：`训练编号-S1` 至 `训练编号-S4`。同一浏览器收到成功或排队回执后，不再重复提交该阶段。
6. 网络失败、超时或服务器拒绝时，本地成绩不会被清空，也不会自动重试；学生可继续答题，稍后重试或改交短码/CSV。
7. 单条 JSON 超过 60,000 字节时停止联网提交，提示改用 CSV，避免碰到服务器单条数据容量边界。

按 240 名学生计算，即使四关都分别提交，单轮最多约 960 条记录；实际只收最终记录时约 240 条。

## QuickForm 主要字段

| 字段 | 用途 |
| --- | --- |
| `protocol` | 固定为 `ALKQF36` |
| `submission_key` | 去重键，如 `AB12CD34EF-S4` |
| `checkpoint_stage` / `checkpoint` | 第几关检查点及中文名称 |
| `class_name` / `student_name` / `student_number` | 班级、姓名、10 位半角数字学号 |
| `session_id` | 本轮随机训练编号 |
| `game_status` / `score` | 进行中或已通关、当前总分 |
| `stage_scores` / `stage_attempts` | 四关分项得分和正式尝试数，用 `|` 分隔 |
| `completed_tasks` / `total_tasks` | 已通过正式任务数 / 13 |
| `formal_attempts` / `hint_count` | 正式尝试总数、提示与救援次数 |
| `wrong_task_ids` / `mastered_wrong_count` | 曾答错任务及已掌握变式数 |
| `active_seconds` | 页面可见且训练进行中的累计活动秒数 |
| `started_at` / `ended_at` / `client_record_at` | 客户端开始、结束和检查点时间 |
| `self_assess` / `coverage` | 学生自评、事件覆盖说明 |
| `event_count` | 轨迹事件条数 |
| `detail_code` | 可由教师解析器还原的完整 `ALK36` 学习轨迹 |
| `detail_bytes` / `payload_hash` | 大小检查与普通传输损坏核对 |

QuickForm 会另外写入服务器的 `submitted_at`。客户端时间和校验码不等于服务器签名，也不能用于正式考试认证。

## 教师端使用

1. 在 QuickForm 任务后台查看或导出提交数据，也可复制 `GET /api/d36lmyvekx/all` 返回的 JSON。
2. 在教师设备打开 `炔烃四类通关游戏_V3.6_教师解析器.html`。
3. 粘贴 `/all` JSON，或选择 QuickForm 导出的 JSON 文件；也可同时导入学生短码和 CSV。
4. 点击“解析全部记录”。解析器先按 `submission_key` 保留较新重复项，再按 `session_id` 合并阶段记录，并优先保留进度最完整的最终记录。
5. 导出“汇总 CSV”用于班级统计，或导出“详细轨迹 CSV”查看具体任务、正式提交、提示、实验运行和变式掌握情况。

教师解析器支持两类 QuickForm JSON：

- `/all` 返回的 `{ "submissions": [...], "total_submissions": N }`；
- 任务迁移包中的 `submissions.json`，其中每项为 `{ "submitted_at": "...", "data": {...} }`。

## 隐私与安全边界

- 学生提交内容包括班级、姓名、学号、成绩、用时、提示/错题和逐次学习轨迹。应只用于本班形成性评价，不公开分享原始 JSON、短码或 CSV。
- 静态网页必须把提交地址写在前端，因此该地址不是密码，不能阻止他人伪造请求。不要把 QuickForm 账号密码或 QFLink 授权码写入学生网页。
- 如 QuickForm 提供“禁止公开读取”开关，建议关闭公开 `/all`，教师改从登录后的任务后台或所有者导出通道取数。
- 超时可能发生“服务器已收到、浏览器未拿到回执”的情况。不要让学生连续点击；教师端用稳定提交键去重。
- 该页面与校验码用于课堂反馈，不提供防作弊、身份认证或不可抵赖证明。

## 更新与部署

把发布包内文件覆盖到原 GitHub Pages 仓库并提交即可。`index.html` 已指向 V3.6；旧版文件仍保留，原网址和二维码不变时可继续使用。部署后请在页面顶部确认显示 `V3.6`，再用一条教师测试身份走到第一关结束；测试完成后可在 QuickForm 后台删除该测试记录。
