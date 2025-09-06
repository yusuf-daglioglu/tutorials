# WEB REST

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 REST (or Representational State Transfer) vs RESTful

Representational kelime anlamı: temsili.

"Representational" kelimesi sunucu tarafta manipüle ettiğimiz kaynakları (state'leri) temsili değerlerle (genelde bir çeşit ID ile) yönetmemizden gelir.

__REST__ yazılım haberleşme tekniği/mimarisidir. "__RESTful__" ise, REST'i kullanan sistemler (uygulamalar) için kullanılır.

REST, 2000 yılında "Roy Thomas Fielding" tarafından bir doktora tezinde ortaya atılmıştır.

REST bir protokol değildir. REST bir protokol mimarisidir. temelde şu kuralların olmasına dayanır:

- __client-server__

  sistem, client'ın herhangi bir platformda çalışabilecek şekilde tasarlanmış olmalıdır.

  __gRPC__: Bu kurala uyar.

  __Websocket__: Bu kurala uyar.

  __HTTP__: Bu kurala uyar.

- __stateless server__

  request'lerde sadece client taraf state(context) ve session bilgisi tutmalıdır. sunucuda tutulmaz.

  __gRPC__: Bu kurala uymaz. Fakat sadece stream yapılan isteklerde uymaz.

  Aşağıdaki Sunucu Tarafı (Bidirectional Streaming) kod örneğinde "responseObserver.onNext(response);" yapmadan 1 satır önce sunucuda global statik bir değişkende veya Redis'te o user'ın bu servise attığı önceki request'lerdeki bilgileri de kullanıp ona göre işlem yapabiliriz. Dolayısı ile server-size'da state tutmuş oluruz.

  ```java
  import io.grpc.stub.StreamObserver;
  import org.lognet.springboot.grpc.GRpcService;
  import com.example.grpc.GreeterGrpc;
  import com.example.grpc.HelloRequest;
  import com.example.grpc.HelloResponse;

  @GRpcService
  public class GreeterServiceImpl extends GreeterGrpc.GreeterImplBase {

      @Override
      public StreamObserver<HelloRequest> BidiHello(StreamObserver<HelloResponse> responseObserver) {
          // Gelen her mesajı asenkron olarak işle
          return new StreamObserver<HelloRequest>() {

              @Override
              public void onNext(HelloRequest value) {
                  System.out.println("Received: " + value.getName());
                  
                  // İşlem sonrasında yanıtı oluştur.
                  // burada uzunca bir business olabilir.
                  // şimdilik test için basit bir kod yazıyoruz.
                  HelloResponse response = HelloResponse.newBuilder()
                          .setMessage("Hello, " + value.getName())
                          .build();
                  
                  // Yanıtı istemciye gönder
                  responseObserver.onNext(response);
              }

              @Override
              public void onError(Throwable t) {
                  System.err.println("Error in stream: " + t.getMessage());
              }

              @Override
              public void onCompleted() {
                  // Akış tamamlandığında yapılacak işlemler
                  System.out.println("Stream completed");
                  responseObserver.onCompleted();
              }
          };
      }
  }
  ```

  __Websocket__: gRPC ile aynı cevap.

  __HTTP__: Bu kurala uyar.

- __Uniform interface__

  her resource için CRUD (Create, update, delete...) metotları hazır olmalıdır.

  __gRPC__: Bu kurala uymaz. CRUD metotları yoktur. Olsa da protokolde tanımlı değildir. Bizim payload'da kendi formatımızı yapmamız gereklidir.

  __Websocket__: gRPC ile aynı cevap.

  __HTTP__: Bu kurala uyar.

- __cacheable__

  client veya aradaki proxy'ler cache'leme yapabilmelidir. her isteğin cacheable olup olmadığı her daim net bir şekilde belirtilmelidir.

  __gRPC__: Bu kurala uymaz. Çünkü bazı istekler karşılıklı streaming yapıyor olabilir. Bunların cache'lenmesi mümkün değildir veya mantıklı değildir.

  __Websocket__: Bu kurala uymaz. sadece payload ile veri paylaşımı olduğu için, tüm payload'ın cache-proxy tarafından dikkate alınması lazım. bu mümkün değil/veya aşırı yorucu (anti-pattern).

  __HTTP__: Bu kurala uyar.

- __layered system__

  sunucularımız birçok katmandan meydana geliyor olabilir. load balancer, proxy, cache, ayrı mikroservisler... bunlar client tarafından bilinmemesi gereklidir. client tek bir endpoint'den tüm ihtiyaçlarını giderebilmelidir.

  __gRPC__: Bu kurala uymaz. "cacheable" kuralında anlatıldığı gibi, gRPC için cache-proxy implemente dilemediğinden dolayı proxy'nin cache yapma olasılığı yok. Belki başka bir görev yapan proxy'ler implemente edilebilir, ama her amaçlı proxy gRPC için mümkün değildir. Dolayısı ile layered bir yapıdan her zaman bahsedilemez.

  __Websocket__: "cacheable" kuralı ile aynı sebepten. sadece payload ile veri paylaşımı olduğu için, tüm payload'ın cache-proxy (veya diğer proxy'ler) tarafından dikkate alınması lazım. bu mümkün değil/veya aşırı yorucu (anti-pattern).

  __HTTP__: Bu kurala uyar.

- __Code-On-Demand__ (optional)

  server, client'a execute etmesi için kod yollayabilir. örnekler: Javascript kodu yada Java-Applet'leri yollayabilir.

  __gRPC__: Bu kurala uymaz. ama opsiyonel olarak server tarafta bunun için kod yazılabilir. ama protokolden bağımsız olarak destekleniyor olabilir.

  __Websocket__: gRPC ile aynı cevap.

  __HTTP__: gRPC ile aynı cevap.

Eğer request/response'larımız bu kurallara uymuyorsa, sistem __RESTless__'dır.

Rest yapısı HTTP ile ortaya atılmıştır ve HTTP ile çok uyumludur. bu sebeple piyasada Rest kelimesi denilince herkesin aklına sadece HTTP gelmektedir. oysa bu doğru değildir.

REST mimarisinde geçen terimler ve buna denk gelen karşılıkları:

| Data Element            | Modern Web Examples                                     |
|-------------------------|---------------------------------------------------------|
| resource                | the intended conceptual target of a hypertext reference |
| resource identifier     | URL, URN                                                |
| representation          | HTML document, JPEG image                               |
| representation metadata | media type, last-modified time                          |
| resource metadata       | source link, alternates, vary                           |
| control data            | if-modified-since, cache-control                        |

## 📌 REST vs SOAP (or Simple Object Access Protocol)

SOAP'ın yeni spesifikasyonlarında kısaltma olduğu bilgisi kaldırıldı ve sadece özel isim olarak kullanılmasına karar verildi.

SOAP, mesajlaşma standartıdır. bu standart mesajın nasıl karşıya gönderileceğine hiç karışmaz. Piyasanın büyük çoğunluğunda HTTP POST isteğinin body'sinde yollanır. Eskiden SMTP protokolü içerisinde kullanılıyordu. Güncel olarak ise Microsoft WebSocket'ler üzerinde sub-protokol olarak kullanıyor "SOAP Over WebSocket Protocol Binding (MS-SWSB)".

SOAP bir protokoldür, oysa REST, protokollerde kullanılan bir tekniktir. Bu sebeple karşılaştırılması doğru değildir. Fakat piyasada REST, sadece HTTP olarak algılandığı için bunun karşılaştırılması yapılıyor.

İstenirse JWT'nin tuttuğu bilgiler SOAP isteğinde tutulur ve SOAP mimarisi ile RESTful bir sistem yapılır. Fakat piyasada kimse bunu tercih etmez. çünkü best practice olarak zaten RESTful sisteme, HTTP request'leri çok uygundur.
