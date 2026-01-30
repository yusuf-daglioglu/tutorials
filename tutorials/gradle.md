# GRADLE

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Gradle

`Groovy` syntax'ı ile build script yazmaya yarıyor (`Groovy` syntax'ı başka başlıkta anlatılmıştır). sadece `JVM` tabanlı projeler için değil, programlama dilinden bağımsızdır. şu anda çoğu eklentisi `Java` tabanlı projeler için yazılığı için öyle bir algı oluşturmuş.

## 📌 Kotlin

`Gradle`, `Groovy`'nin yanında `Kotlin` syntax'ını da destekliyor. dosyanın `Kotlin` ile yazıldığını belirtmek için her `Gradle` dosyasının sonuna `kts` eklemek gerekiyor. örnek:

```
build.gradle.kts
```

## 📌 files

### 📌📌 build.gradle

tüm işleyişin belirlendiği dosya

### 📌📌 gradle.properties

`properties` formatında dosyasıdır. key-value şeklinde `build.Gradle` tarafına parametre geçmek için kullanılır.

### 📌📌 settings.gradle

tüm sub-modüller için geçerli olan key-value şeklindeki property'leri ve işleyişin de belirlenebileceği dosyadır. sub-modüller içeren projede sadece 1 tane (root projede) olması gerekir alt projelerde olmamalıdır.

## 📌 lifecycle

- initialization phase

bu arada Gradle sadece sub-modüllerin var olup olmadıklarını kontrol ediyor.

- configuration phase

__Gradle builds a Directed Acyclic Graph (DAG)__ algoritması kullanarak task'larda döngü olup olmadığını temsil ediyor.

- execution phase

tüm build task'larının çalıştırıldığı fazdır. ilk 2 faz IDE eklentileri tarafından default çağrılırken, bu fazın çağrılması için bir komut gerekmektedir.

## 📌 tasks

Gradle'da fazlar aslında basit yapıdadır. Gradle task'lar üzerine kuruludur. task başka task'ları çağırır. hangi task'ların olacağına plugin'ler karar verir. Java-plugin'i, bazı istisnalar dışında Maven ile aynı isimlere sahip task'lar tanımlamıştır. bu şekilde Maven'a alışmış geliştiriciler, Gradle'daki task'ları da çağırabilir.

build.Gradle içinde;

```groovy
task hello {
   doLast {
      println 'hello world'
   }
}
```

task'ımızı şu şekilde run ediyoruz:

> gradle –q hello

yada

> gradlew hello

Gradlewrapper otomatik olarak task çağırmaktadır.

"gradlew --help" ile "gradlew help" ayrı komutlardır.

Eclipse'te Gradle eklentisi varsa "Gradle tasks" view'ı tüm Gradle task'larını listelemektedir. Gradle task'ları önce proje bazlı gruplandırılmakta daha alt seviyede ise 'task grup'ları bazında listelenmektedir. task'larımızı gruplandırmak istersek; örnek hello task'ımızı bir grup altına alalım:

build.Gradle dosyamızın içine bunu eklememiz yeterli:

```groovy
hello.group = MY_GROUP_NAME
```

Gradle'da hiçbir eklenti yoksa "Help" ve grubu altında bir kaç task varsayılan olarak tanımlı gelir. bunlar Gradle executable'ın içine gömülmüş task'lardır:

- build setup (task grup)

  - init (task)

    boş bir Gradle proje oluşturur. bulunduğumuz dizinde, Gradle.build dosyası vs oluşturur...

  - wrapper (task)

    Gradle wrapper dosyalarını bulunduğumuz dizinde oluşturur.

- help (task group)

  - tasks (task)

    Displays the tasks runnable from root project

  - help

    displays help

  - (and others)

## 📌 task dependsOn

```groovy
task task4(dependsOn: [task1, task3]) << {

   println 'building task4'
}
```

Artık task4 koştuğunda önce task1 sonra task3 koşacaktır. task1 ve task2 koşmuş ise (başka task tarafından) o zaman tekrar koşmaz.

## 📌 :my-sub-project1:myTask

bu şekilde alt projedeki bir task'ı çağırmış oluyoruz. örnek komut:

> gradle :order-microservice:compile

yada aşağıdaki aynı task'ı çalıştırmaktadır:

> gradle order-microservice:compile

## 📌 defaultTasks

defaultTasks'lar eğer task ismi komut satırında hiç verilmemiş ise koşacak task'lardır. örnek:

```groovy
defaultTasks 'clean', 'run'

task clean {
    doLast {
        println 'Default Cleaning!'
    }
}

task run {
    doLast {
        println 'Default Running!'
    }
}

task other {
    doLast {
        println "I'm not a default task!"
    }
}
```

> gradle -q

komutu sadece clean ve run task'larını çağıracaktır.

## 📌 Java plugin

```groovy
apply plugin: 'java'

repositories {

   mavenCentral()
}

dependencies {

   compile group: 'org.hibernate', name: 'hibernate-core', version: '3.6.7.Final'

   testCompile group: 'junit', name: 'junit', version: '4.+'

}
```

## 📌 Java plugin dependency types

- Compile
- Runtime
- Test Compile
- Test Runtime

## 📌 sourceSet

Java plugin'inin sunduğu bir konsepttir. sourceSet; birlikte derlenecek resource ve Java kodlarının bulunduğu dizinlerdir. Örneğin default tanımlı main sourceSet'i:

```groovy
sourceSets {

    main {

        Java {

            srcDirs = ['src/main/java']

        }

        resources {

            srcDirs = ['src/main/resources']

        }

    }

}
```

gibi test source set'i de bulunur. isteğe bağlı ek source set'ler belirlenebilir. örnek: "integration-test" sourceSet gibi.
