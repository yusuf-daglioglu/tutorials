
# JAVA BABEL

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Babel

bir nodejs modülü. bu modül, JS sürümleri arası compile sağlıyor.

Babel config dosyaları:

- .babelrc
- package.json içindeki "babel" anahtarı altındaki ayarlar
- babel.config.js

## 📌 ".babelrc" (JSON) vs "babel.config.js"

".babelrc" JSON iken, "babel.config.js" JS dosyasıdır ve programatik olarak configure edilebilir. 

## 📌 Modules

Babel için birçok nodejs modülü vardır:

- __@babel/core__

JS için API'si sunar. aşağıdaki gibi, JS içerisinde code'u string olarak alıp, kodu çevirebilir.

```js
var babel = require("@babel/core");
//yada
import { transform } from "@babel/core";
//yada
import * as babel from "@babel/core";

//convert code
babel.transform(code, options, function(err, result) {
    result; // => { code, map, ast }
});
```

- __@babel/cli__

Babel'in komut satırından kullanılabilmesi için gerekli nodejs modülüdür.

## 📌 Babel plugins

Babel'de tonlarca eklenti var. örneğin Ecmascript'in her özelliği bir eklenti. bu şekilde convert işlemlerinde özellik özellik enable işlemi yapabiliriz.

eklentiler 2 gruba ayrılıyor:

### 📌📌 Transform Plugins

Belli kod syntax'larını transform ediyor. örnekler:

- @babel/plugin-transform-exponentiation-operator
- @babel/plugin-transform-arrow-functions"

### 📌📌 Syntax Plugins

Sadece parse işlemi yapıyorlar. transform yapamazlar.

Genelde birçok plugin bir arada kullanılır. bunlar gruplanmış şekilde hazır olarak dağıtılırlar. bunu bu şekilde Babel config dosyamızda belirtebiliriz:

```json
{
  "presets": ["es2015", "react", "stage-2"]
}
```

preset değilde, sadece plugin kullanmak istersek yine benzer config'i vardır:

```json
{
  "plugins": ["transform-decorators-legacy", "transform-class-properties"]
}
```

## 📌 plugin and preset rules

Plugin'ler ve preset'ler aynı config'de olabilir. çalışma sırası için kurallar vardır:

- Plugins run before Presets.
- Plugin ordering is first to last.
- Preset ordering is reversed (last to first).

## 📌 @babel/preset-env

bir Babel preset'idir. desteklenen tarayıcı bilgisini config'e gireriz ve bu tarayıcı grubuna uyumlu kod üretir.

örnek kullanım:

```json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
                    "chrome": "58",
                    "ie": "11"
        }
      }
    ]
  ]
}
```

## 📌 Babel 6 "loose" mode

- sadece ES5'e çevirirken kullanılan bir mode'dur.

- Babel uygulaması, ES6 kodunu ES5 koduna çevirirken, daha çok ES6'ya uygun bir şemantik kullanmaya gayret ederek bunu yapıyor.

- this mode normally is not recommended.
