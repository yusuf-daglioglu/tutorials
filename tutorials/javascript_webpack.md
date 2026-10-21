# JAVASCRIPT WEBPACK

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Webpack

Birçok JS, CSS, SASS, HTML gibi birçok web teknolojisi dosyasını birleştirmek (bundle etmek) için kullanılan komut satırı uygulaması. nodejs module uygulamasıdır.

## 📌 webpack-dev-server

Webpack projesi altında olan fakat farklı bir nodejs modülüdür ve ayrı repo'da geliştirilmektedir.

Source code dosyalarını, config dosyasındaki ayarlara göre, server'dan servis olarak sunar.

package.json dosyamıza nasıl run edeceğimizi girelim:

```json
"scripts": {
  "start-dev-server": "webpack-dev-server"
}
```

"webpack.config.js" dosyamıza port, hangi dizini dışarıya sunacağı gibi ayarları gireriz.

Yukarıdakileri config'leri dosyalara girmeseydik, sadece komut satırından da run edebilirdik:

```sh
node "./node_modules/webpack-dev-server/bin/webpack-dev-server.js" --entry "/entry/file" --output-path "/output/path"
# yada
./node_modules/.bin/webpack-dev-server.cmd --entry "/entry/file" --output-path "/output/path"
```

## 📌 Babel-loader

Babel kodu convert ediyor.

Webpack ise, Babel'in çevirdiği bu kodlardan bundle yaratması lazım.

## 📌 hot module replacement (⟷ HMR)

Webpack, JS runtime'ına "module" isminde bir static objeyi variable olarak inject ediyor. static olduğu için JS dosyamızın tepesine import etmeden bu objeyi kullanabiliyoruz.

bir dosya değişikliği sebebi ile hot reload olduğunda, istediğimiz kodu çağırtabiliyoruz.

```js
var requestHandler = require("./handler.js");
var server = require("http").createServer();
server.on("request", requestHandler);
server.listen(8080);

// check if HMR is enabled
if(module.hot) {
  // handler.js dosyası (dependency) update olursa aşağıdaki fonksiyon çalışacak.
  module.hot.accept("./handler.js", function() {

    // requestHandler instance'ımızı tekrardan yaratıyoruz.
    server.removeListener("request", requestHandler);
    requestHandler = require("./handler.js");
    server.on("request", requestHandler);
  });
}
```

## 📌 sourcemap

Web standartlarında olan teknolojidir. sourcemap; minify edilmiş yada JS sürümleri değiştirilmiş JS dosyalarının ilk haline referans edebilmemizi sağlayan dosyalardır. örnek:

minify ve/veya eski sürüme referans edilmiş edilmiş dosyamız:

```js
//# sourceMappingURL=/path/to/script.js.map
Person p = new Person();var a=9;var b=1;
```

Tepesinde sourcemap'e referans var. Bunu yapmazsak, dosyayı çeken "Ajax get" isteğine dönen response'un header'ında şunu koymamız gereklidir:

```text
X-SourceMap: /path/to/script.js.map
```

örnek sourcemap dosyası:

```js
{
    version: 3, // version of sourcemap format
    file: "script.js.map", // name of this file
    sources: [ // orijinal source files
        "app.js",
        "content.js",
        "widget.js"
    ],
    sourceRoot: "/", // optional. root path of "sources"
    names: ["slideUp", "slideDown", "save"], // minify edilen JS dosyasındaki symbol'ler (symbol konusu başka başlıkta anlatılıyor). kaynak: https://sourcemaps.info/spec.html "Proposed Format" başlığında "Line 7" ile başlayan satır.
    mappings: "AAA0B,kBAAhBA,QAAOC,SACjBD,OAAOC,OAAO..." // base64 of orijinal code
}
```

UglifyJS sourcemap yaratmak için geliştirilen bir programdır.

Webpack'deki "development" modu seçeneği, sourcemap dosyalarının oluşturulmasına yarıyor.

gerçek bir örnek:

bu dosya:

```js
import { createBrowserHistory } from 'history';

const history = createBrowserHistory();

export default history;
```

bu halde minified olarak sunuluyor:

```js
__webpack_require__.r(__webpack_exports__);
/* harmony import */ var history__WEBPACK_IMPORTED_MODULE_0__ = __webpack_require__(/*! history */ "./node_modules/history/index.js");
/* harmony import */ var history__WEBPACK_IMPORTED_MODULE_0___default = /*#__PURE__*/__webpack_require__.n(history__WEBPACK_IMPORTED_MODULE_0__);
var history = Object(history__WEBPACK_IMPORTED_MODULE_0__["createBrowserHistory"])();
/* harmony default export */ __webpack_exports__["default"] = (history);//# sourceURL=[module]
//# sourceMappingURL=data:application/json;charset=utf-8;base64,eyJ2ZXJzaW9uIjozLCJzb3VyY2VzIjpbIndlYnBhY2s6Ly8vLi9hcHAvdXRpbHMvaGlzdG9yeS5qcz8wZGE4Il0sIm5hbWVzIjpbImhpc3RvcnkiLCJjcmVhdGVCcm93c2VySGlzdG9yeSJdLCJtYXBwaW5ncyI6IkFBQUE7QUFBQTtBQUFBO0FBQUE7QUFDQSxJQUFNQSxPQUFPLEdBQUdDLG9FQUFvQixFQUFwQztBQUNlRCxzRUFBZiIsImZpbGUiOiIuL2FwcC91dGlscy9oaXN0b3J5LmpzLmpzIiwic291cmNlc0NvbnRlbnQiOlsiaW1wb3J0IHsgY3JlYXRlQnJvd3Nlckhpc3RvcnkgfSBmcm9tICdoaXN0b3J5JztcbmNvbnN0IGhpc3RvcnkgPSBjcmVhdGVCcm93c2VySGlzdG9yeSgpO1xuZXhwb3J0IGRlZmF1bHQgaGlzdG9yeTtcbiJdLCJzb3VyY2VSb290IjoiIn0=
//# sourceURL=webpack-internal:///./app/utils/history.js
```

en alttan ikinci satırda bir self-contained sourcemap dosyası var. o satırı, base64'ten encode edersek elimize şu geçer:

```json
{
    "version": 3,
    "sources": [
        "webpack:///./app/utils/history.js?0da8"
    ],
    "names": [
        "history",
        "createBrowserHistory"
    ],
    "mappings": "AAAA;AAAA;AAAA;AAAA;AACA,IAAMA,OAAO,GAAGC,oEAAoB,EAApC;AACeD,sEAAf",
    "file": "./app/utils/history.js.js",
    "sourcesContent": [
        "import { createBrowserHistory } from 'history';\nconst history = createBrowserHistory();\nexport default history;\n"
    ],
    "sourceRoot": ""
}
```

dikkat edersek; "sourcesContent", ilk dosyamızın her satırını tek tek içeriyor.

## 📌 debugger source list

Firefox -> developer tools --> debugger -> sources --> sol taraftaki liste şu şekildedir:

- main thread
  - localhost:8080
    - main.js
    - utils.js
    - libs
      - JQuery.js
      - backbone.js
  - google.com
    - Google-adds.js
    - Google-utils.js
  - adBlockerFirefoxAddOn
    - page.js
- WebWorker
  - worker.js

Her eklenti, her thread ve her "origin" için dosyalar ayrı ayrı gruplandırılmıştır.

Benzer bir liste Chromium tabanlı tarayıcılarda da vardır.
