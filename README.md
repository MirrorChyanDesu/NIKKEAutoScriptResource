# NIKKEAutoScript Resource

NIKKEAutoScript（NKAS）运行时按需同步的资源仓库，与主项目分开发布，不随主项目更新。
NKAS 在「部署设置 → Resource → ResourceRepository」读取本仓库地址，打开自动主线功能时检查并同步到 `./data/resources`。

## 目录

| 路径 | 内容 |
| --- | --- |
| `version.json` | 整体版本、更新时间，以及各类数据的版本与说明 |
| `models/campaign/` | 自动推图场景、缩略小地图、展开小地图三个 ONNX 模型，及 `bundle.json` 和角色元数据；必须成套分发 |
| `maps/chapter_XX/` | 章节地图运行包：`map.png`、`map.json`、`annotations.json`、`connectivity.json`；附 `source/map_data.npz` 道路概率缓存与地图投影数据 |

模型文件与元数据保持原始字节，模型 SHA-256 记录在对应角色 JSON 中；更新模型须同步版本绑定。

后续新增的数据以 `maps/` 的同级目录加入，并在 `version.json` 的 `components` 中登记。
`scans/` 是 NKAS 扫描任务的本地暂存目录，不提交。

## 版本规则

- 每次数据变更都要提高 `version.json` 的 `version`（以及对应 `components.*.version`）。
- NKAS 有 git 时按提交号判断是否最新，没有 git 时下载 GitHub 分支压缩包并比较 `version` 字符串。
- 本仓库通过 `.gitattributes` 关闭换行转换：地图与模型元数据存在哈希绑定，必须按原始字节分发。
