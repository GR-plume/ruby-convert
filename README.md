## 異なる書式のルビ記法を変換

#### 各書式のルビ記法
- 青空文庫系 `｜漢字《かんじ》` `漢字《かんじ》`
- でんでんマークダウン `{漢字|かんじ}`
- pixiv `[[rb:漢字 > かんじ]]`

### 青空文庫形式 -> でんでんマークダウン形式

`｜漢字《かんじ》` `漢字《かんじ》` -> `{漢字|かんじ}`

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

### でんでんマークダウン形式 -> 青空文庫形式

`{漢字|かんじ}` -> `｜漢字《かんじ》`

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

### でんでんマークダウン形式 -> pixiv形式

`{漢字|かんじ}` -> `[[rb:漢字 > かんじ]]`

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