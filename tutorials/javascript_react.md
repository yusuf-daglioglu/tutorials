
# JAVASCRIPT REACT

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 ReactJS

MVC mimarisindeki V katmanını karşılayan JS kütüphanesidir. çünkü React'ın yaptığı; "React.createElement();" yapıp render edilecek view'ı döndürmektir. business logic olduğunda bunlar farklı JS dosyalarına yazılır. React'taki JS kodları sadece data'nın nasıl görüntüleneceği ile ilgilidir.

## 📌 JSX

JSX dosya uzantısıdır. developer kodları tarayıcıya atmadan, bir transpiller aracılığı ile kodları JS'e çevirir. developer web tarayıcısından debug ettiğinde JS dosyası görür.

JSX, Ecmascript implementasyonu değildir. fakat ek kurallar içererek bazı:
- kolay okunabilirlik
- hızlı yazılabilirlik

gibi özellikler kazandırmaktadır.

JSX sadece React için yazılmış bir syntax değildir. fakat ilk ReactJS ile çıktığından her tarafta bu şekilde anılır. piyasada "React" syntax'ı denildiğinde JSX uzantısı kastedilir. oysa bu algı yanlıştır.

Piyasadaki tüm react örnek kodları minimum Ecmascript 6 (güncel) sürümünü kullanır. fakat istenirse eski sürüm JS'ler ile de React kütüphanesi çağrılıp kullanılabilir. React kodları incelenirken, JS'teki class gibi birçok yeni yapı görülebilmektedir.

Örnek bir JSX dosyası:

```jsx
var Sayac = React.createClass({

  arttir: function () {

        var a = 3;
   },

  render: function () {

    return (

         <div>
          <View style={styles.progressCircleContainer}>
              <Progress.Circle size={50} indeterminate thickness={10} />
          </View>
        </div>
        )
    }
})
```

Yukarıdaki kod tarayıcıda transpiller'dan geçtikten sonra aynı kalır; sadece render'ın içi şu şekilde döner:

```js
return React.createElement(

        "div",

        null,

        React.createElement(

             View,

             { style: styles.progressCircleContainer },

             React.createElement(Progress.Circle, { size: 50, indeterminate: true, thickness: 10 })
        )
```

JSX syntax örnekleri:

```jsx
const element = <h1>Hello, {name}</h1>;

// yada

const element = (<h1>Hello, {name}</h1>);


// yukarıdaki elementi aşağıya yollayabiliriz:
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

```jsx
const element = (
  // bunun içerisi JS'tir. fakat tek satırlıktır. bu sebeple noktalı virgül ile birkaç satır yazamayız.
  // bu JS'in içerisine yazılan kod return edilir ve direk ekranda output olarak gösterilir.
  <h1>

    Hello, {formatName(user)}
  </h1>
);

// Genel JS bilgimize bakarsak;
if(user && user.lastName){ // if içerisinden string dönmektedir fakat bu condition'a sokulduğunda "true" olarak dönüş yapar.
  console.log(user && user.lastName); // if condition'unun içerisindeki ile birebir aynı kod var. burası da string dönüyor. fakat condition içerisinde olmadığından dolayı string kalmaya devam ediyor.
}

// multiline JS yazılamıyor fakat aynı satırda olduğu için ve/veya gibi işlemler kullanabiliriz. örnek:
const element = (
  <h1>
     Hello, {user && formatName(user)}
  </h1>);

// aynı JS syntax'ını XML elementlerine prop eklerken de uyguluyoruz:
const element = <img src={user.avatarUrl}></img>;
```

```jsx
// Ecmascript'in güncel sürümlerinde gelen string içinde dinamik değer kullanmada aynı şekilde JSX'te de beklendiği gibi (değişiklik olmadan) kullanılabilir:
const element = (
  <h1>
     {`hello ${user.name}`}
  </h1>);

// eski sürüm JS'ler ile yukarıdakini bu şekilde yazabiliriz:
const element = (
  <h1>
     {"hello " + user.name}
  </h1>);
```

## 📌 React temel mimari

AngularJS'te olduğu gibi tüm değişkenlerin değişip değişmediğini kütüphanenin kendisi hep kontrol etmiyor. SetState() metodunu yazılımcının çağırması bekleniyor. Bu sebeple performans açısından avantaj sağlanıyor.

React component'lerinde (view'da) bir değişiklik olduğunda, React, komple component'i tekrar render ediyor. Burada performans sorunu ortaya çıkıyor. Fakat bunu çözmek için virtual-Dom mekanizması devreye giriyor. React sanal-DOM üzerinde değişiklikleri render ediyor ve gerçek DOM'a sadece değişiklikleri aktarıyor. buradaki sürece verilen özel isim: __Reconciliation (⟷ Uyumlaştırma)__.

Sayfa render edilirken state değişikliği yapılmaması gerekiyor.

State değişikliklerinde zaten render metodu çalışacağı için sayfa yeniden yeni değerlere göre değişecektir. dolayısı ile yazılımcı update kodlarını yazmaktan kurtulacaktır. AngularJS'te two-way binding olduğundan ekrandaki değişkenler otomatik güncellenir, fakat business logic isteyen kodlar işlenmez. sadece variable'ler güncellenir. AngularJS'te de sayfa initialize olurken, her şeyi parametrik alırsa, AngularJS'te da güncelleme için kod yazmak gerekmez, fakat bu durumda o projede zaten React kullanılması daha mantıklı olacaktır. çünkü React bu felsefe üzerine kuruludur.

__Fiber__, React'ın 16'ıncı versiyonu ile gelen Reconciliation motorudur.

## 📌 page navigation

reactJS'te geçişler single page'dir. web tarayıcısında farklı bir sayfaya gidilmez. fakat rect-native'de bazı routing kütüphaneleri tek activity kullanırken, bazıları her sayfayı farklı activity'lere yönlendirebilir.

React'ta temel olarak routing aşağıdaki şekilde çalışır. About, Users, Home component'leri bulunan sayfaya göre render ediliyor yada edilmiyor. bir routing gerçekleştiğinde tüm sayfa değil sadece Switch'in içi değişiyor.

```jsx
// bu örnek temel mantığı göstermek için yazılmıştır. React-router-DOM ile yazılmıştır.
<Router>
    <div>
      <nav>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/about">About</Link>
          </li>
          <li>
            <Link to="/users">Users</Link>
          </li>
        </ul>
      </nav>

      <Switch>
        <Route path="/about">
          <About />
        </Route>
        <Route path="/users">
          <Users />
        </Route>
        <Route path="/">
          <Home />
        </Route>
      </Switch>
    </div>
</Router>
```

### 📌📌 React-router

routing yapmak için bir kütüphanedir. eğer React-native kullanıyorsak React-router-native paketini de eklememiz şarttır. eğer HTML geliştiriyorsak; React-router-DOM eklememiz şarttır.

DOM ve native paketlerinde aynı arayüzler ile API sunar.

3 ve 4üncü sürümler arasında önemli değişiklikler var.

örnek kullanım:

```jsx
import {
BrowserRouter,
Switch,
Route,
Link
} from "react-router-DOM";

<BrowserRouter>
  <div>
    <nav>
      <ul>
        <li>
          { /* we can use also <a href> here */ }
          <Link to="/">Home</Link>
        </li>
        <li>
          <Link to="/about">About</Link>
        </li>
        <li>
          <Link to="/users">Users</Link>
        </li>
      </ul>
    </nav>

    {/* switch is optional component.
        If we use Switch, only first match will render.
        If we don't use it, then React will render all components
        which match. example:
        if the URL is "/about/company" it will render both "/about" and "/about/company".
    */}
    <Switch>

      <Route path="/about">
        <About />
      </Route>

      <Route path="/users"> { /* or only: <Route path="/users" component="Users"> */ }
        <Users />
      </Route>

      { /* below render is not different from component.
        it allows us to create inline functions.
        this feature called "dynamic routing". it is different then "static routing".
        because dynamic routing allow developers to decide which component will render on runtime. */ }
      <Route
            path={`${match.path}/:name`}
            render={({ match }) => (
              <div>
                {" "}
                <h3> {match.params.name} </h3>
              </div>
            )}
      />

      <Route path="/">
        <Home />
      </Route>

    </Switch>

    { /*
    istersek buraya yukarıdaki ile aynı path'e sahip route tanımlayabiliriz. aynı path olduğundan Ikisi de render olacaktır.
    <Route path="/users">
        <Users />
    </Route>
    */ }

    { /*
    hatta buraya ikinci bir switch daha koyup altına birçok route koyabiliriz:
    <Switch>
      <Route path="/users">
          <Users />
      </Route>
    </Switch>
    */ }

  </div>
</BrowserRouter>

{/* sadece Users component'i içinde nested routing yapalım */}
function Users() {

  let match = useRouteMatch(); // import from "React-router-DOM"

  return (
    <div>
      <h2>Users</h2>

      <ul>
        <li>
          <Link to={`${match.url}/details`}>
            Details of user
          </Link>
        </li>
        <li>
          <Link to={`${match.url}/main_details`}>
            Main details of user
          </Link>
        </li>
      </ul>

      {/* here is nested routing */}
      <Switch>
        <Route path={`${match.path}/:userId`}>
          <UserPhoto />
        </Route>
        <Route path={match.path}>{/* default page */}
          <h3>Please select a link</h3>
        </Route>
      </Switch>
    </div>
  );
}
```

### 📌📌 React-router-Redux (⟷ old-name:Redux-simple-router)

tekrar deprecated oldu, artık __connected-react-router__ paketi kullanılıyor.

React-router'ı wrap ediyor. bu kullanımı opsiyonel olan bir JS kütüphanesidir. Redux state'ine otomatik olarak:

- URL
- URL'ile geçilen path parametreleri

gibi birçok history meta bilgisi ekliyor. bu şekilde development/debug işlerimizi çok kolaylaştırabiliyoruz.

__routerMiddleware__ connected-react-router'ın middleware'idir. kullanılması zorunlu değildir. fakat dispatcher'ın push gibi history işlemleri yapabilmemiz için şarttır. eğer bu olmazsa sadece linkler ile gidilebilir. routerMiddleware initialize olurken parametre olarak history ("history.js" değil! history başka başlıkta anlatılıyor) kütüphanesini istemektedir.

connected-React-router, Redux state'ini değiştirdiği için, state'deki değişikliklerimizi belirleyen reducer'larımıza kütüphanenin reducer'ını eklememiz şarttır:

```js
// reducers.js
import { combineReducers } from 'Redux'
import { connectRouter } from 'connected-react-router'

const createRootReducer = (history) => combineReducers({
  router: connectRouter(history),
  ... // rest of your reducers
})
export default createRootReducer
```

### 📌📌 React-native-router-flux

sadece React-native için alternatif routing kütüphanesidir. React-navigation için farklı bir API sunmaktadır. Bu sebeple kod sadece JS'ten ibarettir.

### 📌📌 React-navigation vs React-native-navigation

Ikisi de sadece React-native için routing kütüphaneleridir.

React-navigation kodunun çoğu JS iken, React-native-navigation'ın çoğu kodu native dillerle yazılmıştır.

#### 📌📌📌 React-navigation

```js

  // App.js

  import { createStackNavigator } from "React-navigation";

  import Home from "./scenes/Home";
  import PushedView from "./scenes/PushedView";

  export default createStackNavigator({
    Home: {
        screen: Home,
        navigationOptions: {
            title: "Home"
        }
    },
    PushedView: {
        screen: PushedView
    }
  });

```

```js
// scenes/Home.js

type Props = {
    navigation: Object
};

export default class Home extends Component<Props> {

    goToPushedView = () => {
        this.props.navigation.navigate("PushedView");
    };

    render() {
        return (
            <View>
                <Button
                    onPress={this.goToPushedView}
                    title={"Push something"}
                />
            </View>
        );
    }
}

```

Yukarıda this.props.navigation.navigate yerine this.props.navigation.push kullanılırsa, aynı sayfalara tekrar gidilmesi sağlanabilir. Aynı sayfalara tekrar gidince aynı activity'den yeni bir tane daha oluşturulmuş olur.

#### 📌📌📌 React-native-navigation

  1 ve 2inci sürümleri arasında çok büyük farklılıklar var.

### 📌📌 NavigatorIOS

sadece iOS için çalışan bir routing kütüphanesi. desteği kaldırıldı.

### 📌📌 createStackNavigator vs createSwitchNavigator

React-navigation'ın (sadece native için geliştirilen routing kütüphanesi) fonksiyonlarıdır.

createSwitchNavigator Authentication süreçleri gibi (örnek login olma) geri tuşunun olmayacağı ve her defasında yeni açılan sayfanın eskisini yok etmesi ile çalışan sayfalarda kullanılır. kullanıcı geri gidememelidir.

createStackNavigator ise geri tuşuna basıldığında bir önceki sayfaya gidilen cinsten routing yapmamızı sağlayan metottur.

bu metotlara sayfalarımızı (component) ve config'lerimiz geçeriz.

### 📌📌 history

history.js (<https://github.com/browserstate/history.js/>) ile bir bağlantısı yoktur.

<https://github.com/ReactTraining/history> repo'sunda geliştirilen pakettir.

Redux ve React-native'den tamamen bağımsız bir JS kütüphanesidir. fakat React-router bu kütüphaneden yararlanarak çalışır.

normal koşullarda web tarayıcısı history'yi tutar. "history" paketi ise JS motoru üzerinde history'nin tutulmasını sağlar ve bize history change listener, change history gibi birçok metodun bulunduğu API sunar.

aşağıdaki import'lar yapılarak history kullanılabilir olur:

`HTML` 5 destekleyen tarayıcılar (diyez olmadan history değişiklikleri sağlar):

```
createBrowserHistory
```

React-native gibi JS motorunun web tarayıcılar tarafından yönetilmediği platformlar için:

```
createMemoryHistory
```

`HTML` 5 olmayan tarayıcılar (diyez işareti ile kullanımı sağlar):

```
createHashHistory
```

`HashHistory`'nin `BrowserHistory`'ye göre avantajı kopyalanabilir linkler üretmektedir. Örneğin; `BrowserHistory` kullanıyor olalım. `/user/detail` sayfamız olsun. Bu sayfaya `React` uygulamamızdan yönlenince (`HTML` 5 standartları gereği) sunucuya `HTTP` `GET` isteği yapmaz. Lokalde (browser'da) bir history oluşturur. `HTML` 5 yenilikleri başlığında bu konu anlatılıyor. Fakat; son kullanıcı `URL`'den kopyalayıp bu linke gitmeye çalışırsa, web tarayıcısı sunucuya `HTTP` `GET` isteği atacaktır. Dolayısı ile sunucuda buna karşılık bir controller olması şarttır. `React` boilerplate projesinde ayağa kalkan `NodeJS` server'ı default olarak bunu yapıyor: gelen herhangi bir isteğe karşılık `index.js`'i döndürüyor. Tabi bu yüklenince otomatik uygulama ilgili path'e routing yapıyor.

örnek kod:

```js
import createHistory from "history/createBrowserHistory";

const history = createHistory();
// Get the current location.
const location = history.location;
```

### 📌📌 React-history

"history" paketi için React component wrapper'ıdır. örnek kod:

```xml
<History>
        {({ history, action, location }) => (

          <p>
            The current URL is {location.pathname}
          </p>
        )}
</History>
```

## 📌 setState()

aldığı ilk parametre bir JSON objesidir. bu obje state'e ekleniyor, var olan değerler varsa update ediliyor. ikinci parametre ise opsiyonel callback metodu.

setState metodu asenkrondur. bu sebeple, setState'e yolladığımız callback metodumuzun da hiçbir zaman senkron çalışmayacağını unutmamak gerekli. JS tek thread çalıştırılıyor. dolayısı ile thread'imiz, yani setState'i çağıran metodumuz bitmeden, callback metodumuz da çağrılmayacak.

Component'in içindeki this.props ve this.state değerleri setState metodu tetiklenip, render işlemi bittikten sonra güncellenir.

dolayısı ile bazı durumlarda; aşağıdakinin yerine:

```js
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

bunu yapmak isteyebiliriz:

```js
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```

Bu bize; render tamamlanmış olmasa da, en son çağrılan setState'in state değerini verecektir.

aynı zamanda React setState işlemlerini kuyruğa alıp sıra ile işlemez. onun yerine hepsini birleştirir öyle render'ı çalıştırır.

```js
handleIncrement = () => {
  this.setState({ count: this.state.count + 1 })
  this.setState({ count: this.state.count + 1 })
  this.setState({ count: this.state.count + 1 })
}

handleDecrement = () => {
  this.setState({ count: this.state.count - 1 })
  this.setState({ count: this.state.count - 1 })
  this.setState({ count: this.state.count - 1 })
}
```

Yukarıdaki handleIncrement metodundaki kod'daki state buna eşittir:

```js
Object.assign(
  {},
  { count: this.state.count + 1 },
  { count: this.state.count + 1 },
  { count: this.state.count + 1 },
)
```

Dolayısı ile bu şekilde yapmalıyız:

```js
handleIncrement = () => {
  this.setState((prevState) => ({ count: prevState.count + 1 }))
  this.setState((prevState) => ({ count: prevState.count + 1 }))
  this.setState((prevState) => ({ count: prevState.count + 1 }))
}
```

## 📌 forceUpdate()

render metodunun çalışmasını sağlıyor. child component'lerinde update olmasını sağlıyor. bu metodun kullanılması önerilmiyor. zaten state yada props değiştiğinde render otomatik çağrılıyor. bu sebeple bu metoda gerek yok. fakat bazen class yada dışarıdaki farklı static değerler render içinde kullanılıyorsa (ki bu hiç önerilmiyor) forceUpdate metoduna ihtiyaç olmaktadır.

## 📌 component objesi metotları (lifecycle)

### 📌📌 sayfa render olurken bir component'in sırası ile aşağıdaki metotları çağrılır

> constructor

burada setState metodu çalışmaz. this.state = {...} state değişkenini direk değiştiririz.

> componentWillMount

bu metot tamamlanmadan render çalıştırılmaz. bu sebeple burada çalıştırılan setState() işlemleri re-render işlemini tetiklemez. componentWillMount içinde çalıştırılan async metotlar bu kurala dahil değildir.

> render( )

pure bir metot olmalı aynı state'e karşılık hep aynı şeyi döndürmeli.

> componentDidMount

Ajax call'ların burada yapılması önerilir. burada setState işlemi re-render'ı tetikler.

### 📌📌 Bir component'in state'inin yada props'unun değişmesiyle tekrardan render edilmesi sonucu çağrılan metotlardır

> componentWillReceiveProps( nextProps )

metodun içinde nextProps vs this.props karşılaştırması yapabiliriz

> shouldComponentUpdate( nextProps, nextState )

eğer false döndürürsek buradan sonra hiçbir metot çağrılmaz.

> componentWillUpdate( nextProps, nextState) )

setState işlemi burada yapılmamalı. eğer şart ise bir önceki metotlar tercih edilmeli.

> render( )

(yukarıda açıklandı)

> componentDidUpdate( )

Ajax işlemleri burada olmalı

### 📌📌 Component yok edilirken çalıştırılacak metot

  > componentWillUnmount()

  obje destroy olduğunda tetikleniyor

## 📌 prop vs state

props; properties'in kısaltmasından gelir.

component'lerde hem prop hem de state isimli JSON'lar vardır. ikisinden biri değiştiğinde render tekrardan çağrılır. bu sebeple bu iki json, ancak setState() gibi metotlarla değiştirilmelidir. çünkü React ancak bu şekilde render'ı çağıracaktır. state.myValue=9; değişikliğini React bilemez, ve render'ı çağıramayacaktır.

prop bir üst component'ten aktarılan event ve variable'lardır. state ise component'in tamamen kendi içinde tuttuğu variable'lardır. props'lar mantıken değiştirilmemelidir. state ler ise; değiştiğinde sayfanın render'ının çalışması gerektiği verileri içermelidir. değiştiğinde sayfa render edilmeyecekse, component class'ının variable'ı tanımlanmalıdır.

bir component prop'ların nereden geldiğini hiç bilmez. component'i çağıran component ona bilgiyi kendi state'inden ⟷ Redux'ın state'inden atmış olabilir. aşağıdan yukarıya data geçilemez (tek istisna: prop ile component'e onEventChange metodu yollarız. ve değişikliği bu metot aracılığı ile manuel isteyebiliriz). bu duruma __yukarıdan-aşağıya (⟷ tek yönlü ⟷ top-down ⟷ unidirectional) veri akışı (⟷ data flow)__ denir.

## 📌 defaultProps

```js
class CustomButton extends React.Component {
  // some code here
}

CustomButton.defaultProps = {
  color: 'blue'
};
```

undefined property'leri için kendini koruyan kod yaratıyor. null prop'lar hala null kalmaya devam eder.

## 📌 props.children

```js
export default class Panel extends React.Component {
  render() {
    return (
      <div>
        <div className="panel-header">{this.props.title}</div>
        <div className="panel-body">{this.props.children}</div>
      </div>
    );
  }
}
```

Yukarıdaki component tanımlandıktan sonra aşağıdaki gibi kullanılabilir:

```xml
<Panel title="Browse for movies">
  <div>Movie 1</div>
  <div>Movie 2</div>
  <div>Movie 3</div>
</Panel>
```

## 📌 propType

aşağıdaki gibi prop'tan geçilecek parametrelerin tipi belirleyebiliriz.

```js
class MyComponent extends React.Component {
     ...
}

MyComponent.propTypes = {

  myValue1: PropTypes.array, //yada PropTypes.array.isRequired

  myValue2: PropTypes.bool,

  myValue3: PropTypes.instanceOf(ClassName),

  myValue4: PropTypes.oneOf(['potato', 'turkey'])
}
```

React'ın eski versiyonlarında propsTypes yerine contextTypes kullanılıyordu.

## 📌 controlled vs uncontrolled component

A controlled component accepts its current "value" as a prop + callback to change that value. example controlled component:

```jsx
<input value={someValue} onChange={handleChange} />
```

Uncontrolled component:

```jsx
<input onChange={handleChange} />
```

Uncontrolled olan versiyonda two-way binding olmayacak.

controlled component daha çok React'a uygun tarzda yazılmış oluyor.

React event'leri component'in kendisinin de define etmiş şekilde sayfayı render etmiyor. React arka planda her event'i tutuyor ve yönetiyor.  örneğin yukarıdaki örneklerde onChange event'lerini direk HTML input'umuz içerisinde göremeyiz.

React'ın DOM'u kendisinin yönetmesinden sebep; beklenmedik durumlarla karşılaşabiliriz. örnek:

```jsx
<input
    inputType="text"
    value="try to change this"
/>
```

Yukarıdaki input son kullanıcı tarafından değiştirilemez olur. çünkü React "value" değerini sabitlemiştir. input'a onChange event'i koysak tetiklenecektir. fakat değer yine değişmeyecektir.

controlled olup olmadığı "value" keyword'ü ile karar veriliyor. tabi component'in onchange tarzı metodu da olmalıdır. onchange'in keyword'ü önemli değil.

"value" keyword'ü bazı HTML objelerinde olmadığı için React render ederken onlara özel olarak ek alanlar/çözümler yaratılmıştır. örnekler:

- input type "text" için; "defaultValue" ile value değeri atayabilmemizi sağlamaktadır.
- textarea'nın normalde (HTML standartlarında) value değeri yoktur. içerisindeki metin textarea etiketinin dışına yazılır. fakat React'ta value kullanılabilirdir.

React'ın kendi DOMlistener'ları vardır. her event'i, native tarayıcı event'lerinden önce React okur, yada sonra yazılımcının yazdığı fonksiyonlara yollar. Yazılımcının yazdığı metotlara event geldiğinde SyntheticEvent gelir. bu even normal event'i wrap etmiş bir objedir. Eğer tarayıcının event'ine ihtiyacımız var ise, gelen event objesinin içindeki nativeEvent isimli "DOMEvent" objesi kullanılabilir.

React event dönüşlerini de hemen tarayıcıya iletmez. arada yine React metodu olacağı için bazı beklenmedik durumlar ile karşılaşabiliriz. örnek: event metodlarımızdan false döndürmek olay yayılımını (propagation'ı) durdurmayacaktır. bu sebeple stopPropagation() gibi fonksiyonları kullanmamız gerekir.

## 📌 componentwillmount mı? componentdidmount mı? Ajax call için uygun?

willmount'ta Ajax call yapılabilir. fakat öneri didmount'ta yazılmasıdır. çünkü:

- client side için sebep: asenkron bir isteğin (örnek Ajax call) ne zaman dönüş yapacağı belirsizdir ve değişkendir. biz kodumuzu geliştirirken render metodunu 2 türlü test etmeliyiz: response-data boşken (daha dolmamış) ve doluyken. fakat eğer testlerimiz/kod geliştirmemiz sırasında Ajax call'arı erken dönüş yapıyorsa; biz response-data boşken (daha dolmamış) durumunu test edememiş olabiliriz. buna ihtimal bırakmamak için Ajax call'arı didmount'ta yapmalıyız.

piyasada yazılımcılar; render işlemi başlamadan request'i yapayım düşüncesindeler. böylece render ederken request gitmiş olur diye düşünülüyor.

- server side için sebep: JSP gibi, React ta istenirse sunucuda derlenip client tarafa atılabilir. sunucu tarafta derlendiğinde componentwillmount metodu hayat döngüsünde çalıştırılmaz. bu sebeple Ajax call'larımızı componentdidmount'de çalıştırmalıyız.

## 📌 container vs component

`component`, `React` sınıfıdır. bu sınıftan türetip tekrar kullanılabilir component'lerimizi oluştururuz. `container (⟷ container component)` ise resmi olmayan bir tanımdır. best practice'lere göre `container`;

- direk GUI objeleri render etmemeli. container'ın render kısmında, component'ler GUI objelerini render etmeli.

- container'lar Redux-store'lara bağlantı kurup component'lere basmaları gereken bilgileri ve handle metotlarını prop olarak vermelidir.

container'lar sayfa veya sayfanın bölümleri (footer, widget gibi) component'lere denir. component'ler ise; textbox'lar, button'lar, text'ler olarak kullanılır.

yine resmi olmayan tanımlarda; container olmayan component'lere "__Dumb (⟷ çöp yığını) component__" veya "__presentational (⟷ sunumsal) component__" adı verilmektedir.

container'lar "__smart component__" olarak da adlandırılırlar. bazıları container'lara "screen" demeyi de tercih edebiliyor. fakat screen, footer component'lerini kapsamamaktadır. bu sebeple screen demek pek doğru olmayacaktır (bu biraz da projenin yapısına bağlı).

## 📌 Higher-Order Components (⟷ HOC)

bu bir pattern'dir.

```jsx
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```

a higher-order component is a function that takes a component and returns a new component.

component'lere prop geçeriz. bu prop'lar ile component'i şekillendirebiliriz. fakat bu prop'lardan bir tanesi de component de olabilir. yani component'i component'e prop olarak geçebiliriz. bu şekilde component direk olarak component'in bir bölgesinde bizim parametre'de geçtiğimiz component'i gösterebilir/render edebilir. Yani; HOC, component parametresi alan component'lerdir. Fakat pattern'de bunu component değilde fonksiyon olarak yapmamız önerilir.

## 📌 fonksiyon call bindings

metotları çağırırken metotların içindeki "this" değerinin undefined olmaması için doğru context'in bind edilmesi gerekmektedir. Çünkü JSX'teki HTML objeleri bizim JS class'ı (component class'ımız oluyor) içindeki fonksiyonu çağırdığında this değeri o sıra JS'teki genel context olacaktır. Oysa biz o fonksiyona; JS class'ımızın context'ini geçmek istiyoruz ki; class içerisindeki state'i props'u kullanabilelim. örnek doğru yöntem:

```jsx
class MyComponent extends React.Component {

  constructor(props) {
    //some code here
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    // Eğer yukarıda gerçek context'i bind etmezsek bu değer bizim istediğimiz değer olmayacak. (peki hangi değer olacak. bu ayrı bir araştırma konusu.)
    this.setState( param );
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```

Hiç bind işlemi yapılmaz bu yapılabilir:

```jsx
<button onClick={(e) => this.handleClick(e)}>
  Click me
</button>
```

Fakat bunun 2 dezavantajı var: (`kaynak: https://en.reactjs.org/docs/handling-events.html "Passing Arguments to Event Handlers" başlığından hemen önceki cümle`)

- her render işleminde bu metot yeni anonim metot generate edecektir
- bu metot HTML-button'a değilde bir React component'ine (örnek MyButtonComponent) prop olarak aktarılsaydı, her render edilişinde yeni anonim metot geleceği için, MyButtonComponent tekrardan render edilecekti.

## 📌 Fonksiyonel bileşen (⟷ functional component) vs class component (⟷ sınıf bileşen)

fonksiyonel:

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

aynı işi göre sınıf bileşeni:

```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

## 📌 React.PureComponent

React.Component'e ile neredeyse tamamen aynıdır. çok ufak farkları vardır. daha hafiftir/performanslıdır.

## 📌 Errors

Exception değildir. JS'te veya JSX'lerde olan hataları yakalamak için kullanılır.

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Bir sonraki render'da son çare arayüzünü göstermek için
    // state'i güncelleyin.
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // Hatanızı bir hata bildirimi servisine de yollayabilirsiniz.
    logErrorToMyService(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // İstediğiniz herhangi bir son çare arayüzünü render edebilirsiniz.
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children;
  }
}
```

bu component'imizi bu şekilde kullanabiliriz:

```jsx
<ErrorBoundary>
    <MyWidget />
</ErrorBoundary>
```

Yukarıda ErrorBoundary, sadece child component'lerde hata olursa hatayı catch edebilecektir.

React 16'dan itibaren, bir hata sınırı tarafından yakalanmamış hatalar, tüm React bileşen ağacının devreden çıkmasına neden olacaktır. Bunun sebebi:

- Messenger gibi bir üründe hatalı bir arayüzün görünür kalması, birinin yanlış kişiye mesaj göndermesine neden olabilir.
- bir ödeme uygulamasında yanlış miktarın görüntülenmesi, hiçbir şey görünmemesinden daha kötüdür.

## 📌 hook

React 16.8'den gelen bir özelliktir. kullanımı opsiyoneldir. functional component'lere benzer ve ek özellikler sunar.

hook, bazı fonksiyonlar sunar. bu fonksiyonlar bazı event'lerde devreye girerler. yani fonksiyonel component'lere ek özellik katarlar. dolayısı ile normal component'lerde kullanılamazlar. direk örnek üzerinden gidersek:

```jsx
import React, { useState } from 'react';

function Example() {
  // "useState" metodu 2 parametre dönüyor:
  //   1- variable'ı tutan değer
  //   2- variable'ı set eden fonksiyon
  // örneğimizde;
  // "count" diyeceğimiz yeni bir state değişkeni tanımladık.
  // count'un ilk değeri 0.
  // "setCount" set eden fonksiyon olsun.
  const [count, setCount] = useState(0);

  // bu kod bloğu örneğinde kullanmadığımız, diğer örnek state'lerimiz:
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

setCount fonksiyonu, setState gibi merge işlemi ile uğraşmaz. direk olarak tüm objeyi/state'i replace eder.

"useEffect" componentDidMount + componentDidUpdate + componentWillUnmount metotlarının tek bir metot'da toplanması olarak düşünülebilir. useEffect hem sayfa ilk init olduğunda ve daha sonraki her render işleminde çalışır. örnek:

```js
import React, { useState, useEffect } from 'react';

function Example() {

  // some code here (from below example)

  useEffect(() => {

    document.title = `You clicked ${count} times`;
  });

  // some code here (from below example)
}
```

useEffect fonksiyonunda return ettiğimiz fonksiyon var ise, component remove olduğunda o fonksiyon çalıştırılır. burada kaynakları serbest bıraktığımız kodlar yazılmalıdır. örnek:

```js
import React, { useState, useEffect } from 'react';

function Example() {

  useEffect(() => {

    // any code here

    return () => {
        // clear resources here!
    }
  });

}

```

useEffect fonksiyonuna ikinci parametre olarak variable geçebiliriz. bu variable'lar değiştiğinde otomatik olarak useEffect çalışacaktır. normalde useEffect sadece componentDidMount + componentDidUpdate + componentWillUnmount'da çalışacaktır. fakat ek olarak bir değer verdiğimizde o değerler değiştiğinde de useEffect çalışır.

## 📌 custom hook

kendi hook'umuzu yaratabiliriz. direk örnek üzerinden gidelim:

```jsx
import React, { useState, useEffect } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log(`The count is ${count}`);
  });

  return (
    <div>
      <p>Count is {count}</p>
      <button
        onClick={() => {
          setCount(count + 1);
        }}
      >
        increase
      </button>
    </div>
  );
}
```

Yukarıda useEffect her render'da çalışır.

eğer bunu yazarsak useEffect sadece bir kere component mount olduğunda çalışacaktır:

```js
useEffect(() => {
    console.log(`The count is ${count}`);
}, []);
```

eğer bunu yaparsak sadece "name" değiştiğinde useEffect çalışacaktır:

```js
useEffect(() => {
    console.log(`The count is ${count}`);
}, [name]);
```

Şimdi gelelim custom hook'lara. useEffect sadece çağrıldığı component'in (fonksiyonel component'in) render'ını takip eder. dolayısı ile dış dünyadan izoledir. bu şekilde state tutan ve kendince render olan fonksiyonlar yaratabiliriz. bunlara hook denir. kendi hook'larımızı "__use__" prefix'i ile yazmamız önerilmektedir.

örnek bir custom hook'u inceleyelim:

```jsx
import React from "react";

// "useUserCollection" bizim custom hook'umuz.
// "useUserCollection" MyComponent içerisinde çağrılıyor. bu sebeple buradaki tüm kodlar sanki MyComponent içindeymişçesine çalışacaktır.
// örneğin useUserCollection içindeki useEffect, MyComponent her render olduğunda çalışacaktır.
const useUserCollection = () => {
  const [filter, setFilter] = React.useState("");
  const [userCollection, setUserCollection] = React.useState([]);

  const loadUsers = () => {
    fetch(`https://jsonplaceholder.typicode.com/users?name_like=${filter}`)
      .then(response => response.json())
      .then(json => setUserCollection(json));
  };

  React.useEffect(() => {
    // bu blok MyComponent her render olduğunda çalışacaktır.
    console.log("useEffect of useUserCollection");
  });

  return { userCollection, loadUsers, filter, setFilter };
};

export const MyComponent = () => {
  // useUserCollection custom hook'umuz bize birden fazla obje döndürüyor.
  const { userCollection, loadUsers, filter, setFilter } = useUserCollection();

  // burası component'imizdeki custom business logic
  React.useEffect(() => {
    loadUsers();
  }, [filter]);

  return (
    <div>
      <input value={filter} onChange={e => setFilter(e.target.value)} />
      <ul>
        {userCollection.map((user, index) => (
          <li key={index}>{user.name}</li>
        ))}
      </ul>
    </div>
  );
};
```

## 📌 React-helmet

nodejs modülüdür. Helmet component'ini sunar. Helmet component'i içinde render edilen HTML objeleri, header'a koyulurlar (eğer zaten önceden header'da aynı etiketler var ise onları override eder).

örnek:

```jsx
import React from "react";
import {Helmet} from "react-helmet";

class Application extends React.Component {
  render () {
    return (
        <div className="application">
            <Helmet>
                <meta charSet="utf-8" />
                <title>My Title</title>
                <link rel="canonical" href="http://mysite.com/example" />
            </Helmet>

            ..
            <input type="text" defaultValue="hello world">
            ..
        </div>
    );
  }
};
```

## 📌 React-native

Mobil platformlar için geliştirilmiş bir framework'tür. Bu çatıda React-js syntax'ı ile (sınıfların isimleri aynı. örnek: component) JS kodları, React-native modülü sayesinde mobil OS'ların native kodlarına çevrilmektedir. Cordova gibi yapılarda webview üzerinde çalışan arayüz, React-native'de native arayüzde çalışmaktadır.

Cordova ile aynı şekilde parse işlemi yapılmaktadır. resource dosyalarında bulunan JSX dosyaları, runtime sırasında native arayüze çevrilmektedir. native component'lerin çevrilmesi az da olsa bir süre almaktadır. bu açıdan bakıldığında Cordova ile bir fark yoktur. fakat daha sonra native component'ler, WebView'dakilerden daha hızlı çalışmaya devam etmektedirler. işte Cordova'ya göre performans açısından avantaj sağladığı nokta budur.

## 📌 JS Engine on runtime

JavaScriptCore Safari'nin açık kaynaklı JS motorudur. JavaScriptCore iOS'ta zaten yüklüdür. Android'de ise React-native bunu kendi içine gömer.

JSX'ler compile sırasında JS'e çevrilir. Bu kodlar ise runtime'da JavaScriptCore engine ile çalıştırılır. JavaScriptCore React-native içerisinde gelmekte ve uygulamanın runtime'ına eklenmektedir. JavaScriptCore ile çalışan uygulama, native modül geldikçe native tarafta işlem yapmaktadır. örneğin; bir JSON objesi, yada bir integer native tarafta tutulmamaktadır. ancak ve ancak native modül ihtiyacı (kod satırı native modülü çağırıyorsa) native tarafta işlem gerçekleştirilmektedir. tüm işlemler JS'te yapılmaktadır + render edilen sayfalar native modüller çağırarak UI render ederler. fakat o native UI'ların event'leri tekrar JS core'da işlem yaparlar. örneğin onPress'teki JS olan "i++" kod satırı, JavascriptCore'da çalıştırılır.

Google-Chrome ile debug edilen native uygulama daki JS engine, Google-Chrome'un içindekidir. Bu sebeple production ortamımızdaki react-native build'u ile bazı farklılıklar yaşayabiliriz. Google-Chrome ile Android içerisinde debug edilen uygulama WebSocket aracılığı ile haberleşmektedir.

## 📌 Reconciliation (⟷ uyumlaştırma)

React altyapısı arka planda tüm sayfayı render etmez. sadece gerekli alanları render eder. böylece performance artışı sağlar. burada bir hesaplama yapılması gereklidir. hangi objelerin update olup olmayacağı algoritmasına Reconciliation denir.

`React`; __virtual DOM (⟷ VDOM)__ oluşturur ve `VDOM` ile gerçek `DOM`'u karşılaştırarak `Reconciliation` yapar.

## 📌 Virtual DOM for React-native

React-native'de HTML DOM'u olmadığından nasıl çalışıyor? React-core içerisindeki yapıda virtual DOM görevini gören sistem çok esnek tasarlanmış. programlama dilinden bağımsız. dolayısı ile render'dan return edilen değerler ile var olan view karşılaştırılıyor. bu karşılaştırma sonucu sadece değişen kısımlar, o andaki sayfaya uygulanabiliyor.

## 📌 React activity

React uygulaması bir activity'dir. manifest'te belirttiğimiz (veya ana sayfamızı yapmak zorunda değiliz) main-activity'miz ReactActivity'den türemiş bir activity olmalıdır. uygulama bunu çağırdığında React o activity için devreye girecektir.

React yapısı gereği activity dışında Android'in uygulama hayat döngüsünde de bir kaç setup'u vardır. Hatta uygulamaya ek olarak ReactApplication'dan da extend işlemi yapmalıyız. (Android'in Application hayat döngüsü başka başlıkta anlatılıyor)

## 📌 native koda erişme

React-native'de hem native koddan JS'e hem de tersi mümkündür. fakat erişim işlemi asenkrondur.

## 📌 native-base

React-native GUI elementlerinden extend edip kullanmamızı sağlayan React-native için geliştirilmiş bir modülüdür. yani bir katman daha eklenerek daha fazla nitelik kazandırılmaya çalışılıyor. aynı JSP->JSF katmanları gibi. native-base özellikle yeni event'ler/nitelikler değilde; hem iOS hem de Android'de kendi platformlarına göre daha oturaklı component yaratmak için tema(theme) felsefesinde geliştirilmektedir.

## 📌 React-devtools

bu isim ile web tarayıcıları için eklenti mevcut. bu eklenti tarayıcıya kurulduğunda, tarayıcının developer tool'una ek bir sekme geliyor. bu şekilde sadece React-js debug'ı kolaylaşıyor. normalde sadece web tarayıcısı debug için yeterlidir. fakat bu eklenti inspect element işini COmponent bazlı yapıyor/gösteriyor ve her elemente geçilen prop'ları gösteriyor.

React-devtools ismi aynı olsa da bağımsız bir npm modülüdür. electron tabanlı bu npm modülü, React-devtools eklentisini barındırıyor.

## 📌 React-native-debugger

React-devtools tabanlı bir debugger. ekstra bazı özellikler içeriyor. örnek Redux support.

## 📌 hot reload vs live reload

live reload'da proje JS dosyalarında herhangi bir değişiklik olduğunda emulator'deki uygulamamız otomatik komple restart olur ve state'ini kaybeder.

hot reload'da ise uygulama restart olmaz ve state'ini kaybetmez. fakat değişiklikler RAM'de çalışan uygulamaya yansır. dolayısı ile değişikliği görebilmemiz için ilgili JS kod bloğunun tekrar işletilmesi yeterli olacaktır.

yukarıdakiler sadece JS dosyaları için geçerlidir. diğer dosyalar için uygulamayı tekrardan komple run etmek gerekir.

## 📌 npm start -- --reset-cache

sadece "npm start" yaptığımızda JS dosyaları bundle halinde hazır hale getirilir ve HTTPüzerinden sunuma açılır. JS dosyalarında bir değişiklik olduğunda ise bu bundle baştan sona işlenmez sadece değişiklik olan kısım hemen bundle'de güncellenir. npm start komutu iptal edilip tekrar çalıştırıldığında npm tekrar bundle'yi üretmez. eskisini kullanır. fakat "-- --reset-cache" komutu ile bundle tekrardan sıfırdan üretilir.

"npm start", NodeJS sunucusu ayağa kaldırıyor ve aşağıdaki gibi public URL'leri dışarıya açıyor:

<http://localhost:8081/index.ios.bundle>  //bundle JS'i komple veriyor

<http://localhost:8081/index.android.bundle>

<http://localhost:8081/debugger-ui.html> //buradaki sayfada webWorker (JS thread teknolojisi) kullanılıyor. Bu sayfa packager (npm start ile başlatılan uygulama) tarafından sağlanıyor. packager WebSocket teknolojisi ile  ile haberleşiyor.

emulator'de yada cihazda uygulamamızı başlattığımızda, emulator gidip kodları <http://localhost:8081/index.ios.bundle'dan> çekmesi gerekir. packager'ın IP:port bilgisini mobil cihazımızın "React dev menu"sünde set etmemiz gerekir.

emulator/cihaz ve debugger(örnek: web tarayıcısı) uygulamaları NodeJS sunucusuna; yani "npm start" ile başlattığımız packager'a bağlanıyorlar. artık ikisi(debugger ve emulator) packager'i proxy olarak kullanarak haberleşiyor. eğer haberleşme başlar ise; web tarayıcısı da index.android.bundle dosyasını kendine packager'dan indiriyor. artık JS dosyası emulator'den değil, web tarayıcısının kendisinde koşmaya başlıyor.

## 📌 metro bundler

React-native'in bundle hazırlamasını yarayan ve npm modülüdür. "npm start" yaptığımızda bu çalışır.

## 📌 ref

"ref" her component'e daha hızlı erişebilmemizi sağlayan bir fonksiyonalite sağlar. React'ta eskiden şu şekilde kullanım vardı:

tanımlama kodu:

```html
<input ref="myInput">
```

erişim için kod:

```js
this.refs.myInput
```

Daha sonra bu şekilde kullanım önerilmeye başlanmıştır:

tanımlama kodu:

```jsx
ref={(input) => { textInput = input; }}
```

erişim için kod:

```js
this.textInput
```

## 📌 React için bazı best practice'ler/notlar

- bir component çok fazla prop almaya başladığında çok fazla özelleşmeye başlamış anlamına gelir. bir süre sonra, her prop için if blokları component içinde artar. bu da karmaşıklığın artmasına sebep olur. bu sebeple; belli bir süre component'ler klonlanarak/kopyalanarak ayrı bir component haline getirilir. çünkü çok özelleştirme abartılırsa, yeni bir component olması gerektiği anlamına gelmektedir.

- bir liste elemanları için birer obje oluşturduğumuzda, listenin eleman sayısına göre/index'e göre key kullanmamamız şarttır. hatalı kullanıma örnek:

```jsx
const todoItems = todos.map((todo, index) =>
  // Bunu yalnızca öğelerinizin sabit ID'leri yoksa yapın
  <li key={index}>
    {todo.text}
  </li>
);
```

key=index yerine key=todo.id kullanmalıyız. çünkü listeye daha sonradan yeni bir eleman eklenirse, liste index'e göre hazırlandığı için hatalar meydana gelecektir.

"key" React için özel bir keyword'dür. key random da atanmamalıdır. çakışma olursa hata olacaktır. ID veya key her zaman atamak zorunda değiliz, fakat listeye elemen eklenecekse, çıkarılacaksa (yani re-render) edilecek ise mutlaka key olmalıdır.

- React component'leri birbirinden extend etmemeleri (class extends yöntemi) önerilir. onun yerine birbirini kapsayan (composite) component'ler tercih edilmelidir. önerilen kullanım aşağıdaki gibidir:

```jsx
<MyCustomComponent> <AnotherComponent> ... </AnotherComponent> </MyCustomComponent>
```

"composition over inheritance" başlığında anlatıldığı gibi bu durum React içinde geçerlidir. var olan data sadece prop'lardan ve kendi state'imizden gelmelidir. diğer bilgiler bizi ilgilendirmemelidir. eğer React component'lerimiz inheritance olursa, super class'tan gelecek objelerinde variable'larına göre kod yazmamız gerekir.

dikkat edilirse Redux kullanırken mapToProps ile Redux store'undan ihtiyacımız olan değerleri prop olarak kendi component'imize bağlarız. dolayısı ile Redux'ın store'u React'tan bağımsız bir yapı olmasına rağmen, props aracılığı ile component içine çektik. böylece focus'umuz sadece props ve state değerleri olmuştur. kompleksliği azaltmış oluyoruz.

- tüm hibrit uygulamalarda olduğu gibi Native tarafın UI Thread'i ile JS motorunun thread'i farklı olduğundan bu ikisinin haberleşmesi de çok yavaştır. özellikle döngü içinde her seferinde native kod çağırmak çok maliyetlidir.

- React setState içerisinde === kontrolü ile değişiklik olup olmadığına bakar.  eğer değişiklik varsa sadece değişikliğin parametre olarak yollandığı component'leri render eder. sebeple business logic'ler render içinden kaldırılıp, render metodu içindekiler component'lere bölünerek, business logic'ler oralara taşınmalıdır. bu şekilde istatistiksel olarak daha az kod işlenecektir. zira belki o component hiç tekrardan render edilmeyecek ve business logic kısmı dahi çalıştırılmayacaktır. fakat business logic component dışında olsaydı render metodunun başlaması ila çalıştırılıyor olacaktı.

   örnek;

   ```jsx
   render(){
     if(state.hesaplar == true){
          <div> state.accounts </div>
     }
   }
   ```

   re-render edildi. if kontrolü yapıldı. oysa hesaplar değerinde hiç değişiklik yoktu. o zaman aşağıdakini yazmak daha mantıklı olacaktır:

   ```jsx
   render(){
      <HesaplarComponent value={state.hesaplar} accounts={state.accounts}/>
   }
   ```

- state'i bir value'ya atayıp değiştirip sonrada o value'yu state'e set etmek yanlış. aşağıdaki örnek yanlış:

   ```jsx
   var x = this.state.x;

   x.z.push("foo"); // z is a list

   this.setState({x:x});
   ```

   burada olabilecek sorunlar:

  - React setState metodunu işletirken state'i değişmemiş sanabilir. çünkü içini biz zaten değiştirdik (o anda ilgili tüm componentShouldUpdate gibi metotları incelemek gerekir)

  - paralel bir thread state'i kullanıyor/değiştiriyor olabilir. state'i elle değiştirmemiz bir çakışmaya yol açabilir.

  bu sebeple bunun yerine Object.assign kullanıp obje klonlanmalıdır. listeler içinse [].concat([ newElement1, newElement2 ]) kullanmalıyız.

- React render veya içindeki farklı bir component'i render edip etmeyeceğine karar vermek için deep comparison yapıyor. yukarıdaki notta state'i elle değiştirmekten bahsettik. eğer elle değiştirseydik ve hemen ardından setState çağırsaydık bu bizim zaten dezavantajımıza olabilirdi. çünkü Immutability React'ta çoğu zaman avantaj sağlıyor. çünkü genelde state'in içinden bir eleman değiştirildiğinde zaten component'in render olması gerekecek. fakat React önce state'in tümünü dolaşarak buna karar veriyor. fakat React dolaşırken, daha az dolaşmasını sağlamalıyız. bu sebeple en tepedeki bir objenin klonunu o obje yerine yerleştirdiğimizde, React direk olarak en tepeden kontrol etmeye başlayacağı için değişikliği görecek ve render işlemine hemen başlayacak. yani React framework'ünün compare işlemini kısaltmış olacağız. tabi bu her zaman geçerli olan bir durum değil. örneğin;

   state = {x, y} olsun. render metodunuzun içinde sadece Y'yi kullanan 100 adet component olsun. 1 adet component'te Y içindeki A değerini kullansın. sadece A yı değiştirmek istersek, object assign ile Y'yi komple yeni obje olarak klonlayacağımızı için 100+1=101 adet component tekrar render olacaktır. oysa sadece Y.A yı değiştirseydik, sadece 1 component render olacaktır. yani object assign her zaman avantaj sağlamayabilir.

- loading bar

  paralelden işlem yapılırken loading bar gösterilir.

  ```jsx
  render(){

   if(state.loading){

      <LoadingBar>

   } else {

      <SCREEN>
   }
  }
  ```

  yerine;

  ```jsx
  render(){
      <LoadingBar hidden=state.loading>
      <SCREEN>
  }
  ```

  yapılmalıdır. çünkü state.loading değiştiğinde sadece zaten render olmuş bir bar hide edilecektir. diğer türlü state.loading değiştiğinde tüm sayfa render edilecek.

- render metodu içinde business logic hiç olmamalı. render'ın görevi sadece sayfayı oluşturmaktır. logic'lerle ilgilenmek farklı bir görev.

- render metodu içinde anonim metot olmamalı. çünkü render'ın sürekli çağrılacağı düşünülerek uygulama tasarlanmalı. anonim metotlar runtime sırasında oluşturulur. bu da zaman kaybı yaratır. anonim metot yerine; önceden bir kere metot oluşturulmalı ve o metot kullanılmalıdır.

  aslında bu kural genel olarak her şey için geçerli fakat genelde her taraftaki anonim metotları sabit metot olarak ayarlamak kodun daha kötü okunmasına sebep oluyor. buna normal koşullarda tölerans gösterebiliyoruz. fakat render metodu sürekli çalışacağı için burada bu konuda tölerans göstermemek gerekiyor.

## 📌 "React-native link" komutu

/root/node_modules/ dizini altında native platform bağımlı kütüphaneler olabilir. burada bulunan kütüphanelerin /root/ios/ ve /root/android/ altındaki projelere import edilebilmesi için bu komut kullanılır. istenirse komutsuz manuel de import yapılabilir.

## 📌 React-native-cli vs React-native

React-native-cli komut satırından React-native projesi oluşturma gibi komutları yerine getirmek için yapılmış, ilgili projenin dependency'lerinde olmak zorunda olmayan bir nodejs modülüdür.

React-native ise React-native'in runtime'da çalıştırması gereken bir nodejs modülüdür.

## 📌 React vs React-dom vs React-native

React-native çıkmadan önce sadece ReactJS vardı. ReactJS, React paketi ile dağıtılıyordu. React-native çıkışı ile birlikte "React" paketi her platform için "React-core" paket oldu. web development yapanlar React-dom'u, mobile geliştiricileri is React-native'i de package.json'a eklemelidirler.

## 📌 RNPM (⟷ React-native package manager)

eskiden kullanılan fakat artık React kütüphanesinin içine entegre edilen nodejs modülüdür. komut satırından paket kurulumları, link işlemlerini yapabilmemizi sağlıyordu.

## 📌 React dizin yapısı

- / - root dizini

- /android - Gradle root projesi. burası Android Studio için proje dizini. Eclipse için workspace dizini.

- /android/settings.gradle - burada npm içindeki modüllerin native Java kodlarının dizinleri mevcut. bu şekilde her proje IDE tarafından workspace'te görülebilir olacaktır. bu sebeple Android Studio açıldığında bir sürü proje görünmektedir.

- /android/app - Android Studio için Android modül dizini. Eclipse için Android proje dizini.

- /android/app/build.gradle - dependencies kısmında compile('com.Facebook.React') ile JAR'lar direk Gradle network'ünden indirilmektedir. yine dependencies kısmında "compile project(':React-native-code-push')" satırları mevcut. bu projeler bir üst dizindeki settings.Gradle'da workspace'e tanıtılmıştı.

- /android/app/src/main/java - Java kodları

- /android/app/src/main/AndroidManifest.xml

- /android/app/build/.../index.android.bundle - bütün /app dizinindeki JS dosyaları burada tek dosya haline getiriliyor ve apk içine taşınıyor. Android runtime'da bu dosyayı okuyor.

- /ios

- /app - React JS kodları ve kaynak dosyaları

- /index.android.js - android projesinde başlayacak olan ilk sayfa (yeni React init'le gelmiyor)

- /index.ios.js - ios projesinde başlayacak olan ilk sayfa (yeni React init'le gelmiyor)

- /index.js - ilk açılacak dosya

- /package.json - libs

- /node_modules - npm yöneticisinin dosyaları

## 📌 create-react-app

<https://github.com/Facebook/create-react-app> repo'sundan dağıtılan, bir ReactJS projesi oluşturup ayağa kaldırmamızı yarayan node-modülüdür. proje ilk kurulduğunda sadece 3 bağımlılık vardır:

```json
//package.json file
"dependencies": {
  "react": "^16.11.0",
  "react-dom": "^16.11.0",
  "react-scripts": "3.2.0"
},
"scripts": {
  "start": "react-scripts start",
  "build": "react-scripts build",
  "test": "react-scripts test",
  "eject": "react-scripts eject"
},
```

React-scripts modülü diğer tüm işleyişi halledecek script'lerimizi içerir: node server'ı kaldırabilir ve dışarıya projemizi web tarayıcısı üzerinden açabilir, projedeki değişiklikleri otomatik takip edip hot reloading yapabilir, ES kodlarını eski sürümlere göre build eder, production ve dev ortamlarına göre derleme yapabilir...

## 📌 <https://github.com/react-boilerplate/react-boilerplate>

bu boilerplate hem create-React-app'in yaptığını yapıyor, hem de proje içindeki dosyaları da oluşturuyor. yani; her component, container, Redux ve saga, i18n için gerekli tüm dosyaları da oluşturuyor.
