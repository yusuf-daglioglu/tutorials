############################

############################
# JAVASCRIPT FLUX REDUX SAGA
############################

############################

# Flux

Türkçe kelime anlamı: akış, akıntı

Flux client side uygulamalar için mimarisel bir pattern'dir.

Facebook, Flux pattern'ini çıkardığında, aynı isimde bir de Javascript implementasyonu ile birlikte çıkardı. piyasada birçok Flux pattern'i uygulayan Flux implementasyonu bulunmaktadır.

# Flux vs MVC
Flux pattern'i temel olarak MV*'lere alternatiftir.

Flux'ın akışı bu yöndedir:

```
Action --> Dispatcher --> Store --> View
```

Dispatch Türkçe kelime anlamı: sevk etmek. yazılım dünyasında metota/fonksiyona mesaj(parametre) yollama(sevk etme) anlamında kullanılır.

Flux, en çok MVC (version-1)'ye aşırı benzerdir. Hatta Flux; daha detaylı hale getirilmiş bir MVC (version-1) türevidir. Aslında MVC'yi gerçekten nasıl daha implemente edeceğimizi anlatır. Çünkü pure MVC (version-1) zaten yine tek yönlüdür. Fakat Facebook ilk Flux'u ortaya attığında MVC'deki en temel problemin MV*'lerde; M, V ve C arasında gidiş dönüş bazen karmaşık döngü yaratabildiğini savundu. Oysa pure MVC'de tek yönlü (or unidirectional) bir hareket vardır. Fakat uzun zamandır birçok MVC türevi çıkmıştı ve kimse en optimum şekilde pure-MVC'yi implemente etmedi. Yıllarca MVC terimi daha çok; M, V ve C'yi kategorize etmek (ayırmak) için kullanılan bir terim oldu. Dolayısı ile; Facebook kendi MV* türevini çıkarıp buna "Flux" ismini verdi.

# Redux vs Flux
Bakıldığında Redux, Flux pattern'inin biraz değişik bir implementasyonu olduğunu görebiliriz.

Redux, Flux'tan farklı olduğundan; terminolojide Redux'a, "Redux akışı", "Redux pattern"i uyguladığı yazılır.

- Redux tek bir store kullanır. Flux'ta birden fazla store olabilir. Aslında Redux'ta da multiple store kullanabiliriz. yani; bağımsız yeni Redux-store generade edip kullanabiliriz. fakat bu önerilmez. çünkü Redux'ta "reducer" mantığı vardır. buna gerek kalmamaktadır. (belki çok uç/istisna durumlar için kullanmak gerekebilir)
- Redux'tan Flux'taki gibi merkezi bir "dispatcher" konsepti yoktur. Redux'ta reducer metotları vardır.
- Redux reducer metotlarında her zaman yeni obje döndürülmesini bekler. dolayısı ile data'mızın hiçbir zaman manipüle edilmediğinden emin olunur. (bu uygulanması zorunlu olan bir durum değil, fakat yapılmaması, Redux'ın mantığına aykırı olacağı için anlamsız olur) (bu konu "Redux store immutability"'de daha detaylı açıklanmıştır.)

# Redux store immutability
- Redux'ı IDE/Web tarayıcı tool'larında debug ederken, "time-travel debugging" şeklinde debug edebilmeyi amaçlarız. bu durumda reducer metotlarının side effect'inin olmaması beklenir. reducer'lar state'i güncellemesi gerekli. bu durumda reducer'ların ancak ve ancak, yeni state'i dönerlerse side effect'siz olurlar. bu sebeple state immutable olmalıdır.

- kodun tasarımını/okunaklılığını kolaylaştırır. State'in hiç değişmediğini bilerek geliştirmek işi çok basitleştirecektir.

- React-Redux kütüphanesinde data değişiklikleri "shallow equality"ye dayanır. dolayısı ile root elementinin yeni bir referansa sahip değilse; "connect" data değişikliğini algılayamaz ve re-render işlemi gerçekleşmez.

"Redux vs Flux" başlığındaki 3'üncü madde'de belirtilen isteğe bağlı state'in manipüle edilebilmesi durumu şudur: state'i reducer'larımızda düzenleyebiliriz/manipüle edebiliriz. fakat bu kesinlikle tavsiye edilmez.

# Redux
Türkçe kelime anlamı: canlandırma (örnek: ekonomi canlanması)

Javascript kütüphanesidir.

tüm uygulamada ortak bir store kullanılmasını sağlamaktadır. bu store'un her taraftan ortak metotlarla yönetilebilmesini sağlamaktadır. store'un şeması (içinde hangi bilgilerin yer alacağı), uygulamanın o andaki state'ine göre önceden tanımlıdır. sadece verilerin içeriği o sıra uygulama tarafından doldurulur/kullanılır. bu sebeple Redux Predictable (önceden kestirilebilir) bir storage'dir.

Redux genel kullanımı:

storu değiştirmek için yapılacak olan bir __aksiyon (or action)__ vardır. örneğin bu aksiyonumuz bu olsun:

```json
{
  "type": "ADD_TODO",
  "text": "Go to school"
}
```

Yukarıda JSON'da:
- type: aksiyon tipimiz
- text: aksiyonumuzun içeriği

Aksiyon alacak kişi bu tarz bir JSON'ı göndereceğini önceden bilemez (bilmesi gerekmez). onun yerine bize bu JSON'u döndüren bir metot yazarız.

```js
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}
```

İşte bu yukarıdaki metot "__Action Creator__"'dır. Bunun gibi bir sürü metotumuz olacaktır.

Normalde beklenen: addTodo metotunun en son satırında storage'a kayıt atması olacaktır:

```js
function addTodo(text) {
  const action = {
    type: ADD_TODO,
    text
  };
  dispatch(action);
}
```

fakat Redux'ta bunu yapmaya gerek yok. onun yerine şu yeterlidir:

```js
import { createStore } from 'Redux'

// createstore tüm uygulamadan bir kerelik yapılıyor.
// hangi parametrelerin geçileceği sonraki adımlarda anlatılacak. şu adımda önemli değil.
const store = createStore(...);

// dispatch fonksiyonu sync çalışır ve response olarak son state'i döndürür.
// saga gibi middleware'ler kullanıldığında, middleware ne zaman dönüş yaparsa o zaman dispatch dönüş yapacaktır. kodumuz dispatch'in ne döndürdüğüne değil, store'a bağladığımız listener'lara göre aksiyon almalıyız. çünkü saga gibi middleware'ler yield ile çalışır. bu sebeple, başlatacağımız bir işlem birçok başka işlemi tetikler. ve bunlar yield sayesinde asenkron olurlar. dolayısı ile dispatch state'in en son halini dönmez.
store.dispatch(addTodo(text));
```

Geldik bu aksiyona göre veritabanını (storage'yi) değiştirmeye... Değişiklikleri __reducer (or reducing function)__ metotlarımız ile yapıyoruz.

reducer kelime anlamı: redüktör (indirgeyici)

```js
function todoApp(currentState = initialState, action) {
   switch (action.type) {
     case ADD_TODO:
        return {
            text: text
            ...currentState
        }
     default:
        return currentState;
  }
}
```

Yukarıdaki metotta currentState aslında currentStorage'dir. Fakat genelde/örneklerde hep state olarak yazılıyor.

Yukarıdaki state, Redux tek bir storage kullandığında her tarafta aynıdır.

switch case'deki default durumu; bilmediğimiz bir aksiyon ile gelinirse storage'de hiçbir şey değiştirmeyiz. bu bir hata durumudur aslında.

ADD_TODO gelirse; şimdiki state'e text diye bir property daha eklemiş oluruz.

currentState = initialState Ecmascript 6 ile gelen bir syntax özelliğidir. currentState null gelirse initialState ile doldururuz. bu şekilde uygulama ilk açıldığında buraya gelme durumunda storage'yi initialize eder.

reducer'den döndürdüğümüz(return ettiğimiz) nesne, bizim artık yeni state'imizdir.

tahmin edildiği gibi her reducer metotunun tüm state'i görerek işlem yapması sakıncalıdır. bunun için her reducer'a parametre olarak tüm state değil, state'in sadece ilgili parçası gönderilir. örneğin şöyle yaptığımızı düşünelim:

```js
function reducerMethods(state = {}, action) {
  return {
    a: reducerA(state.a, action),
    b: reducerB(state.b, action),
    c: reducerC(state.c, action)
  }
}
```

Yukarıdaki bu işlemi Redux kolaylaştırıyor. __combineReducers__ isimli metot sunulmuş. şu şekilde örnekleyebiliriz:

```js
import { combineReducers } from 'Redux'

const reducerMethods = combineReducers({
  a: reducerA,
  b: reducerA,
  c: reducerC
})
```


# genel metotlar

```js
const store = createStore(reducer, initialState);
```

artık bu store üzerinde işlem yaparken sadece bu reducer çağrılır.

```js
store.getState()
```

tüm state'in son halini döndürür

```js
store.dispatch(action)
```

alınan aksiyon JSON'u ile reducer metotu çağrılır. reducer'ları kombine etmek zorundayız. eğer kombine etmezsek tek bir reducer olacağından metot çok büyük/uzun olur.

# store vs state
store aşağıdaki başlıkta anlatıldığı gibi bir obje iken, state sadece data'nın kendisini temsil etmek için kullanılan terimdir.

# store objesi
createstore fonksiyonu ile yaratılan Redux store'umuz ile döndürülen JS objesi bu fonksiyonları içerir:

```js
type Store = {
  dispatch // dispatch fonksiyonu
  getState // uygulamamızın son state'ini dönen fonksiyon
  subscribe // state değişiklikleri için atadığımız listenler'ları bu fonksiyon ile register ediyoruz
  replaceReducer // yeni reducer kullanmak istediğimizde bu fonksiyona onu yollamak zorundayız.
}
```

# enhancer
Redux'ın core'unda olan bir özelliktir. bu özellik eklenti altyapısına benzer. Redux createstore fonksiyonuna istediğimiz enhancer'larımızı geçeriz. createstore fonksiyonu, bize enhancer'daki kurallara göre farklı bir çeşit store objesi döner.

enhancer genelde sayfa yazan yazılımcılar tarafından yazılmaz. genelde kütüphaneler/framework'ler enhancer geliştirirler.

örneğin Redux-devtools-extension'ı runtime'da web sayfasının JS objesin bu fonksiyonu inject eder:

```js
window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__
```

Biz bu fonksiyonu parametrelerle çağırıp enhancer instance'ımızı oluşturabiliriz:

```js
import { createStore, applyMiddleware, compose } from 'Redux';

var composeEnhancers;

if( window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ != undefined){

    composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__( { } ); // şimdilik boş parametrelerle çağıralım.
} else {
    // "compose" is the Redux's default enhancer
    composeEnhancers = compose;
}

// enhancer, diğer tüm middleware'leri wrap eder:
const enhancer = composeEnhancers(
  applyMiddleware(...middleware),
  // other store enhancers if any
);
const store = createStore(reducer, enhancer);
```

Böylece eklenti redux store'unu takip edebilecek ve araya girip değişiklik dahi yapabilecek.

# middleware vs enhancer
Middleware, action'lar üzerinde işlem yapar. her action çalışdığında devreye girerler.

enhancer, store üzerinde yapılandırma değişikliği yapar. örneğin, store'un LocalStorage'de saklanmasını yönetmesi gibi...

# Redux middleware

Redux core içinde bulunan bir özelliktir. dispact işlemine başlamadan önce ve sonra istediğimiz callback metotlarını atabilmemizi sağlar. örneğin her dispatch öncesi ve sonrası state'imizi console'a basmak istersek bir logger-middleware'i yazarız.

middleware ile yapabileceğimiz herşeyi elle dispatch metotunu wrap ederek te yapabiliriz.

basit bir middleware örneği:

```js
// parameters passing to middleware automatically by Redux.
// dispatch => store's dispatch
// getState => store's state
// this function will be call every call of "dispatch" method
function loggerMiddleware({ dispatch, getState }) {
  return next => action => {

    console.log('store before dispatch' + getState());
    let returnValue = next(action); // this is expected behavior on most middlewares.
    console.log('store after dispatch', getState());
    return returnValue;
  }
}
```

Yukarıdaki yazım kodu Ecmascript 5 ve sonrası olduğu için farklı görünebilir. ilk satırlar aslında şunu yapıyor:

```js
function loggerMiddleware({ getState, dispatch }) {
  return function (next) {
    return function (action) {
```

Yukarıdaki middleware'imizi createStore'a parametre olarak yollamamız gerekmektedir:

```js
let store = createStore(
  reducers,
  initParams,
  applyMiddleware(logger-middleware)
);
```

eğer birden fazla middleware'miz var ise bunları applyMiddleware metotuna parametre geçmemiz gerekiyor:

```js
applyMiddleware(logger-middleware, middleware2, middleware3 );
```

middleware'ler içerisinde sadece next metotu ile diğer middleware çağrılmak zorunda değil. middleware içinde aynı zamanda getState.dispatch() metotunu kullanabiliriz. çağıracağımız dispatch metotu ile paralelden yeni bir aksiyon zinciri başlatılacaktır. dispatch metotu senkron çalışmaktadır. bu sebeple yeni zincir thread olarak başlamayacaktır.

# React-Redux
Redux'un React ile bütünleşik kullanılmasını sağlayan kütüphanedir. bu paket olmazsa, state yenilendiğinde o anda yürütülen react-component'leri otomatik render edilmez. React'ta event bazlı çalışma şart olduğundan, bu modülün mantık olarak kullanılması şarttır.

React-Redux eklentisinin sunduğu bir özellik "__connect__" metotudur. bu metot aracılığı ile React'ın component'inin props'una Redux-state'inden bir obje geçebiliyoruz ve dispatch metotlarımızı da component içine kullanıma açabiliyoruz.

dispatch ve state property'lerimiz React component'lerine props olarak geçilmektedir.

# Redux Thunk vs Redux Promise vs Saga
3'ü Redux için middleware görevi görür. Redux üzerinde asenkron işlemleri kolaylaştırırlar. asenkron işlemler için kullanılması zorunlu değildir. fakat büyük/kompleks uygulamalarda kolaylık ve standart sağlar.

# Redux thunk
thunk'ın terminolojik açıklaması başka başlıkta anlatılıyor.

middleware'dir. Redux middleware'in kodu çok kısadır:

```js
function createThunkMiddleware(extraArgument) {
  return ({ dispatch, getState }) => (next) => (action) => {
    if (typeof action === 'function') {
      return action(dispatch, getState, extraArgument);
    }
    return next(action);
  };
}

const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;

export default thunk;
```

Koduna bakarsak yaptığı iş şudur:
- Eğer action bir fonksiyon değilse; hiçbir şey yapmaz, normal süreci işletmeye devam eder, yani; dispatch'i kendisi çağırır.
- Eğer action bir fonksiyon ise; bu fonksiyonu çağırır. Çağırırken ona dispatch, state ve middleware initialize edilirken yollanan ek parametreleri yollar.

kullanımı:

```js
const INCREMENT_COUNTER = 'INCREMENT_COUNTER';

function increment() {
  return {
    type: INCREMENT_COUNTER,
  };
}

// yukarıdaki 1'inci case'e düşen kullanım örneği:
dispatch(increment());
```

kullanımı için farklı bir örnek:

```js
function actionCreator(param1) {
  return function(dispatch, getState) {
    return asyncFetch().then(
              (responseData) => dispatch(actionCreator2()),
              (error) => dispatch(actionCreator3()),
    );
  };
}

// yukarıdaki 2'inci case'e düşen kullanım örneği:
store.dispatch(actionCreator('arg1'));
```

Thunk middleware yukarıdaki örn ekte olduğu gibi async işlemler için veya store'a erişmek için kullanılması önerilen bir middleware'dir.

# Saga
saga kelime anlamı: destan

async caller'ı çok kolaylaştıran bir middleware'dir. thunk'tan çok daha gelişmiştir.

bir saga örneğine bakalım:

```js
import { call, put, takeEvery, takeLatest } from 'redux-saga/effects'

function* fetchUser(action) {
   try {
      const user = yield call(Api.fetchUser, action.userId);
      yield put({type: "USER_FETCH_SUCCEEDED", user: user});
   } catch (e) {
      yield put({type: "USER_FETCH_FAILED", message: e.message});
   }
}

function* mySaga() {
  yield takeEvery("FETCHING_DATA", fetchUser);
  yield takeLatest("FETCHING_IMAGE", fetchImage); // "takeLatest" saga'nın en önemli yeteneklerinden biri. bunu framework'süz yapmaya kalksak çok zorlanırız.
}
```

# Redux araçları

Redux'un araçlarını kullanabilmek için uygulamamızın kodları içine kod eklemeliyiz.

- Redux-devTools-extension

  web tarayıcıları için Redux debug tarayıcı eklentisi.

- Redux-devtools

  tarayıcılardan bağımsız çalışabilen Redux debug modülü.

# (Redux + saga) vs (React hooks + React context)
'React context'i, static global store olarak kullanılmaktadır. bu da Redux'ın store'una alternatif gelmektedir. react-hook'lar ise, Redux ve saga'nın sunduğu connect metotlarına alternatif/benzer görev yapmaktadır. fakat saga'ya full bir alternatif değildir. çünkü kompleks süreçlerin yönetimini, React hooks ve React context ile yapamak için, elle bir çok aşamayı yönetmek için kod yazmamız gerekir.

React hooks ve React context native olduklarından, 3üncü parti kütüphane ihtiyacı ve entegrasyon (config) yapmak gerekmemektedir.

Basit uygulamalarda React hooks ve React context (ikisi birlikte) tercih edilebilir. Fakat aksiyonların çok olduğu, birçok farklı isteğin üstüste yapıldığı, paralel async isteklerin çok olduğu sayfalarda Redux ve saga (ikisi bilikte) tercih edilmelidir. Çünkü özellikle saga middleware'i, gerçek büyük bir saga işlemini ve hatta paralelden çağrılan saga'ların yönetimini kolayca sağlamaktadır. örneğin; takeLatest gibi özellikler saga çok kolay bir şekilde sunmaktadır.
