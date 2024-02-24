## 異なる書式のルビ記法を変換

### 青空文庫形式のルビ記法をでんでんマークダウン形式に変更

```js
const reg = /｜([々〆〇〻\u2E80-\u2FD5\u3192-\u319F\u3400-\u9FFF\uF900-\uFAFF]|[\uD840-\uD87F][\uDC00-\uDFFF])+《[^《》]+》|([々〆〇〻\u2E80-\u2FD5\u3192-\u319F\u3400-\u9FFF\uF900-\uFAFF]|[\uD840-\uD87F][\uDC00-\uDFFF])+《[^《》]+》/g

function convert(str) {
  const matched = str.match(reg)
  let _str = str
  matched.map(e => {
    const splited = e.split(/《|》/)
    const kanji = splited[0].replace('｜', '')
    const ruby = splited[1]
    const dendenRuby = `{${kanji}|${ruby}}`
    _str = _str.replace(e, dendenRuby)
  })
  return _str
}
```

### でんでんマークダウン形式のルビ記法を青空文庫形式に変更

```js
const reg = /{[^{}\|]+\|[^{}\|]+}/g

function convert(str) {
  const matched = str.match(reg)
  let _str = str
  matched.map(e => {
    const splited = e.split(/\|/)
    const kanji = splited[0].replace('{', '')
    const ruby = splited[1].replace('}', '')
    const aozoraRuby = `｜${kanji}《${ruby}》`
    _str = _str.replace(e, aozoraRuby)
  })
  return _str
}
```

### でんでんマークダウン形式のルビ記法をpixiv形式に変更

```js
const reg = /{[^{}\|]+\|[^{}\|]+}/g

function convert(str) {
  const matched = str.match(reg)
  let _str = str
  matched.map(e => {
    const splited = e.split(/\|/)
    const kanji = splited[0].replace('{', '')
    const ruby = splited[1].replace('}', '')
    const pixivRuby = `[[rb:${kanji} > ${ruby}]]`
    _str = _str.replace(e, pixivRuby)
  })
  return _str
}
```