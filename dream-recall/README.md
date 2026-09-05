# 梦境寻忆 · 在线模板

WOS 助手「小工具 → 梦境寻忆 → 关卡模板 → 在线模板」读的就是这个目录。

## 目录结构

```
dream-recall/
  index.json              模板目录，应用启动这一页时取它
  packages/<slug>.zip     模板包，与 vFlow 导出的格式一致（template.json + reference_image.*）
  thumbs/<slug>.jpg       缩略图，列表里那一张小图（长边约 480px）
```

## index.json

```jsonc
{
  "schemaVersion": 1,          // 应用只认 1；改结构时才加，加了旧版本会整份拒绝
  "updatedAt": "2026-09-05",   // 仅供人看
  "templates": [
    {
      "id": "…",               // 模板包 template.json 里那个 id，应用据此判断"已导入"
      "name": "海塔",
      "slug": "haita",         // 仅供人看；文件名用它，应用不读
      "itemCount": 50,
      "referenceWidth": 1080,
      "referenceHeight": 2400,
      "sizeBytes": 981492,     // 必须与 zip 实际体积一致，对不上应用会中断下载
      "sha256": "…",           // 必须是 zip 的 SHA-256，对不上应用会删掉重来
      "thumbnail": "dream-recall/thumbs/haita.jpg",
      "download": "dream-recall/packages/haita.zip"
    }
  ]
}
```

`thumbnail` 与 `download` 只能是**仓库内的相对路径**：应用会拒绝绝对地址、`..` 以及
路径之外的字符，那是它唯一挡着"清单把用户引到别处"的地方。

## 加一个模板

1. 把模板包放进 `packages/`，缩略图放进 `thumbs/`（长边 480px 左右即可）。
2. 在 `index.json` 里补一条，`sizeBytes` 与 `sha256` 按实际文件填：
   `shasum -a 256 packages/<slug>.zip`
3. 提交推送。应用侧无需发版——目录是运行时取的。

改了某个模板的内容，`sha256` 必须跟着改。用户重新下载会**覆盖**同一个关卡（`id` 相同），
不会多出一份。
