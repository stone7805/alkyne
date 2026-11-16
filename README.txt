炔烃四类通关游戏 V3.6｜GitHub Pages 发布说明

1. 解压 ZIP，把本文件所在文件夹里的全部文件上传到原 alkyne 仓库根目录；不要只上传 ZIP，也不要再套一层文件夹。
2. index.html 是 V3.6 学生端；parser.html 是 V1.2 教师解析器。
3. V3.6 接入 QuickForm：https://quickform.cn/api/d36lmyvekx 。每关完成后由学生勾选同意并主动提交，不会自动上传；断网仍可答题并导出 ALK36 短码或 20 列 CSV。
4. 教师解析器支持 QuickForm /all JSON、任务迁移 submissions.json、ALK34—ALK36 短码和 V3.5/V3.6 CSV；会按提交键去重，再按训练编号合并阶段记录。
5. 详细字段、隐私边界、教师取数步骤和验收结果请阅读 README.md 与 验收报告_V3.6.md。
6. 旧版 HTML 继续保留，原 GitHub Pages 网址不变时，二维码无需更换。部署后请确认页面顶部显示 V3.6。

开发验收没有向正式 QuickForm 接口写入测试数据。首次投放前，请由任课教师用明确标记的测试身份实际提交第一关，确认后台收到后再删除该测试记录。
