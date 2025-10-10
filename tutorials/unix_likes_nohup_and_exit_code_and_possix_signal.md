# UNIX LIKES NOHUP AND EXIT CODE AND POSIX SIGNAL

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 nohup

POSIX için komut satırı yazılımı. aldığı parametreleri komut olarak arka planda çalıştırtır. böylece komut satırı kapatıldığında uygulama çalışmaya devam eder. nohup'a alternatif yazılımlar da mevcuttur. MS Windows için "start" komutu benzer görevi görür.

## 📌 nohup vs disown vs &

  Aşağıdaki 3 örnekte 'commandX' yeni process'e atanır ve shell yeni komut alacak şekilde kendini ayarlar.

  aşağıdaki her işlemde de commandX'e sterr, stdin gibi stream'ler bağlanır.

| command usage                    | standart input (stdin)                      | standard output (stdout) and standard error (stderr)                        | job listesinde tutulur     | SIGHUP sinyalini                |
|----------------------------------|---------------------------------------------|-----------------------------------------------------------------------------|----------------------------|---------------------------------|
| commandX param1 param2 &         | program buraya data yollarsa hata alacaktır | hala terminal'e bağlıdır, bu sebeple durduk yere ekranda output görebiliriz | process hafızada tutuluyor | process'e yollar                |
| commandX param1 param2 \| disown | program buraya data yollarsa hata alacaktır | program buraya data yollarsa hata alacaktır                                 | process hafızadan silinir  | process'e yollanmasını engeller |
| nohup commandX param1 param2     | kapatır ve process'e EOF karakteri gider    | redirects to nohup.out                                                      | process hafızada tutuluyor | process'e yollanmasını engeller |

  Bazen programlar disown yada nohup'la başlatmamıza rağmen shell kapandığında process'te kapanır. bunun sebepleri:

- shell kapatılınca her process'e SIGHUP sinyali yollanır. bu sinyal sonrası process kapanmak zorunda değildir. process istediği gibi sinyali handle edebilir. çoğu zaman programlar kapanmayı tercih eder.

- shell kapandığında stin, stdout gibi stream'ler null olduğundan NullPointer gibi hatalar alan program hata fırlatarak beklenmedik durum gereği kapanır. bu sebeple bazıları stout'u > file.txt gibi bir dosyaya yönlendirmemizi önerir.

## 📌 job control

POSIX'lerde shell'ler arka plana attıkları yazılımları job listesinde tutar. bu sisteme "job control" denir.

terminal'in her başlattığı process 'child process'tir.

job control, temel olarak "process group" teknolojisini kullanarak çalışır.

"jobs" komutu o anda terminal'de çalışan arka plan uygulamalarını print eder.

## 📌 POSIX sinyalleri

POSIX standartları altında tanımlanmıştır. çalışan process'lere veya bir thread'e özel birçok çeşit sinyal gönderilebilir. bu sinyaller 64 tanedir. C'de signal.h içerisinde de tanımlıdırlar. Bazı sinyaller:

- SIGHUP - 1 numaralı sinyal - "signal hang up" yada "HUP" da deniliyor. terminal'de çalışan process, terminal kapatıldığında bu sinyal ilgili process'e yollanır.

- SIGINT - 2 numaralı sinyal - komut satırında Ctrl+C tuşu aynı görevi işler. Bu yazılımcı tarafından handle edildiğinde göz ardı edilebilir. yani program kapanmak zorunda değil.

- SIGKILL - 9 numaralı sinyal - Uygulamayı yazılımcı handle edemeden kapatır. Core dump dosyasını saklar.

- SIGSTOP - 18 numaralı sinyal - komut satırında Ctrl+Z tuşu aynı görevi işler - uygulamayı pause eder.

- SIGCONT - 19 numaralı sinyal - uygulamayı pause durumundan resume ile devam ettirir.

Bazı sinyaller piyasada kullanılmakta fakat POSIX standartlarında belirtilmemiştir.

Diğer sinyaller:

kaynak: (source-id: 65)

```c
#define  SIGHUP 1  /* hangup */
#define  SIGINT  2  /* interrupt */
#define  SIGQUIT  3  /* quit */
#define  SIGILL  4  /* illegal instruction (not reset when caught) */
#ifndef _POSIX_SOURCE
#define  SIGTRAP  5  /* trace trap (not reset when caught) */
#endif
#define  SIGABRT  6  /* abort() */
#ifndef _POSIX_SOURCE
#define  SIGIOT  SIGABRT  /* compatibility */
#define  SIGEMT  7  /* EMT instruction */
#endif
#define  SIGFPE  8  /* floating point exception */
#define  SIGKILL  9  /* kill (cannot be caught or ignored) */
#ifndef _POSIX_SOURCE
#define  SIGBUS  10  /* bus error */
#endif
#define  SIGSEGV  11  /* segmentation violation */
#ifndef _POSIX_SOURCE
#define  SIGSYS  12  /* bad argument to system call */
#endif
#define  SIGPIPE  13  /* write on a pipe with no one to read it */
#define  SIGALRM  14  /* alarm clock */
#define  SIGTERM  15  /* software termination signal from kill */
#ifndef _POSIX_SOURCE
#define  SIGURG  16  /* urgent condition on IO channel */
#endif
#define  SIGSTOP  17  /* sendable stop signal not from tty */
#define  SIGTSTP  18  /* stop signal from tty */
#define  SIGCONT  19  /* continue a stopped process */
#define  SIGCHLD  20  /* to parent on child stop or exit */
#define  SIGTTIN  21  /* to readers pgrp upon background tty read */
#define  SIGTTOU  22  /* like TTIN for output if (tp->t_local&LTOSTOP) */
#ifndef _POSIX_SOURCE
#define  SIGIO  23  /* input/output possible signal */
#define  SIGXCPU  24  /* exceeded CPU time limit */
#define  SIGXFSZ  25  /* exceeded file size limit */
#define  SIGVTALRM 26  /* virtual time alarm */
#define  SIGPROF  27  /* profiling time alarm */
#define SIGWINCH 28  /* window size changes */
#define SIGINFO  29  /* information request */
#endif
#define SIGUSR1 30  /* user defined signal 1 */
#define SIGUSR2 31  /* user defined signal 2 */
```

Yazılımcı kod içerisinde bir listener aracılığı ile bu sinyalleri handle etmesi gerekir. C üzerinden örneklersek:

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <signal.h>

void sighandler(int);

int main () {

   // signal.h'taki bir fonksiyondur.
   // ilk parametresi: hangi sinyal için handle çalıştırılsın
   // ikinci parametresi: handler fonksiyonumuz
   signal(SIGINT, sighandler);

   while(1) {
      printf("sleep for a second...\n");
      sleep(1);
   }
   return(0);
}

void sighandler(int signum) {
   printf("Caught signal %d, coming out...\n", signum);
   exit(1);
}
```

Sinyaller process'imize iletildiğinde, process thread'i beklemeye alınır ve handler'ımız execute edilir. handler bittiğinde main process devam eder.

bir handler çalışırken, başka bir sinyal daha gelirse, önce ilk handler tamamlanır, sonra diğer handler için tekrar aynı handler devreye girer. 2inci handler bittiğinde main fonksiyonu çalışmaya devam eder. aşağıdaki kod ile bunun testini output kısmını inceleyerek görebiliriz. CTRL+C ile çalışan uygulamaya sinyal gönderildiğinde, uygulamanın çıktısı değişmektedir.

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <signal.h>

void sighandler(int);

int main () {
   signal(SIGINT, sighandler);
   int mainLoopId = 0;
   while(1) {
      mainLoopId++;
      printf("main sleep -- mainLoopId=%d\n", mainLoopId);
      sleep(1);
   }
   return(0);
}

int signalHandlerId = 0;

void sighandler(int signum) {
   signalHandlerId++;
   int loopId = 0;
   while(loopId<5) {
      printf("handler sleep -- signalHandlerId=%d loop=%d \n", signalHandlerId, loopId);
      sleep(1);
      loopId++;
   }
}
```

### 📌📌 Kimler ve hangi durumlarda sinyal alınıp/yollanabilir

- İşletim sitemi arka planda bu sinyalleri yazılımlara yollayabilir. örnek:
  - son kullanıcı OS'u kapatmak için event açsın. bu durumda OS uygulamaların tümüne kapanmaları için özel bir sinyal gönderir.

- Yeterli yetkisi olan son kullanıcı bir process'e sinyaller gönderebilir. örnek:
  - kill, signal komut satırı uygulamaları ile process-ID'sini parametre olarak geçtiğimiz process'e, istediğimiz sinyali gönderebiliriz.

- yazılımcı kod içerisinden sinyal yollayabilir. örnek:
  - C'de; signal.h içindeki __int raise(int sig)__ fonksiyonu ile kendi işlemimize sinyal gönderebiliriz.
  - C'de; __int kill( pid_t pid, int sig )__ ile diğer process'lere sinyal gönderebiliriz.

## 📌 exit status (or exit code)

program kapandığında bir integer sinyal döner. bu sinyale verilen addır.

Java'da; System.exit(int status) metodu ile yapılır.

bir shell fonksiyonundan dönen return değer ile, shell üzerinden çalıştırılan bir programın exit code'u aynı statüdedir.

fakat çoğu shell implementasyonu bir komut işlettiğinde, ilgili komut ne döndürürse onun 255 ile mod işlemini alıp döndürüyor.

- 0 başarılı bittiği anlamına gelir.

- pozitif sayılar daha çok yazılımcı tarafından yakalanmış/yönetilmiş hatalardır.

- negatif sayılar yazılımcının yakalamadığı/yakalayamadığı durumlarda döndürülür.

Bazı exit statüler standarttır. örneğin bazı status'ler POSIX standartlarında belirtilmiştir. örnek:

code - açıklama

1 - programmatic error example: 1/0 divide zero

2 - binary file format example: missing keyword. for example calling a function which is not on classpath

126 - Command invoked cannot execute. example: file is not executable

127 - command not found to execute

128 - exit code integer değilse o zaman bu hatayı verir.

128+x - Örnek: "kill -9 $PID" komutu çalıştırıldığında 9 sinyali programa yollanır. Eğer program bunu yakalayamazsa, program 128+9=137 fırlatır. Burada 128 şu sebepten var: İlk 128 kod piyasada sıkça kullanılıyor. Bunları override etmemek için OS'larda böyle bir yönteme gidilmiş.

Yukarıdaki değerler yazılımcı tarafından da fırlatılabilir. fakat bu hiç önerilmez. best practice değildir.

Çoğu child process açan API (örnek C language API) child process kill olunca nax 128'lik bir değer dönecek şekilde yazılmışlardır. Dolayısı ile 128 üstü değerler döndürebiliriz fakat bunu kimin kullanacağı önemlidir. public API yazdıysak max-128 kuralına uymak iyi olur.

C ve C++ dillerinde standart kütüphanelerinin (sysexits.h) içinde aşağıdaki değerler vardır. fakat bunlar OS'un yada başka programlama dillerinin ve hatta diğer c kütüphanelerinin resmi olarak tanıdığı standartlar değildirler.

<https://raw.githubusercontent.com/openbsd/src/refs/heads/master/include/sysexits.h> dosyasının içeriği aşağıdadır:

```c++
#ifndef _SYSEXITS_H_
#define _SYSEXITS_H_

/*
 *  SYSEXITS.H -- Exit status codes for system programs.
 *
 * This include file attempts to categorize possible error
 * exit statuses for system programs, notably delivermail
 * and the Berkeley network.
 *
 * Error numbers begin at EX__BASE to reduce the possibility of
 * clashing with other exit statuses that random programs may
 * already return.  The meaning of the codes is approximately
 * as follows:
 *
 * EX_USAGE -- The command was used incorrectly, e.g., with
 *  the wrong number of arguments, a bad flag, a bad
 *  syntax in a parameter, or whatever.
 * EX_DATAERR -- The input data was incorrect in some way.
 *  This should only be used for user's data & not
 *  system files.
 * EX_NOINPUT -- An input file (not a system file) did not
 *  exist or was not readable.  This could also include
 *  errors like "No message" to a mailer (if it cared
 *  to catch it).
 * EX_NOUSER -- The user specified did not exist.  This might
 *  be used for mail addresses or remote logins.
 * EX_NOHOST -- The host specified did not exist.  This is used
 *  in mail addresses or network requests.
 * EX_UNAVAILABLE -- A service is unavailable.  This can occur
 *  if a support program or file does not exist.  This
 *  can also be used as a catchall message when something
 *  you wanted to do doesn't work, but you don't know
 *  why.
 * EX_SOFTWARE -- An internal software error has been detected.
 *  This should be limited to non-operating system related
 *  errors as possible.
 * EX_OSERR -- An operating system error has been detected.
 *  This is intended to be used for such things as "cannot
 *  fork", "cannot create pipe", or the like.  It includes
 *  things like getuid returning a user that does not
 *  exist in the passwd file.
 * EX_OSFILE -- Some system file (e.g., /etc/passwd, /var/run/utmp,
 *  etc.) does not exist, cannot be opened, or has some
 *  sort of error (e.g., syntax error).
 * EX_CANTCREAT -- A (user specified) output file cannot be
 *  created.
 * EX_IOERR -- An error occurred while doing I/O on some file.
 * EX_TEMPFAIL -- temporary failure, indicating something that
 *  is not really an error.  In sendmail, this means
 *  that a mailer (e.g.) could not create a connection,
 *  and the request should be reattempted later.
 * EX_PROTOCOL -- the remote system returned something that
 *  was "not possible" during a protocol exchange.
 * EX_NOPERM -- You did not have sufficient permission to
 *  perform the operation.  This is not intended for
 *  file system problems, which should use EX_NOINPUT or
 *  EX_CANTCREAT, but rather for higher level permissions.
 * EX_CONFIG -- Something was found in an unconfigured or
 *  misconfigured state.
 */

#define EX_OK  0 /* successful termination */

#define EX__BASE 64 /* base value for error messages */

#define EX_USAGE 64 /* command line usage error */
#define EX_DATAERR 65 /* data format error */
#define EX_NOINPUT 66 /* cannot open input */
#define EX_NOUSER 67 /* addressee unknown */
#define EX_NOHOST 68 /* host name unknown */
#define EX_UNAVAILABLE 69 /* service unavailable */
#define EX_SOFTWARE 70 /* internal software error */
#define EX_OSERR 71 /* system error (e.g., can't fork) */
#define EX_OSFILE 72 /* critical OS file missing */
#define EX_CANTCREAT 73 /* can't create (user) output file */
#define EX_IOERR 74 /* input/output error */
#define EX_TEMPFAIL 75 /* temp failure; user is invited to retry */
#define EX_PROTOCOL 76 /* remote error in protocol */
#define EX_NOPERM 77 /* permission denied */
#define EX_CONFIG 78 /* configuration error */

#define EX__MAX  78 /* maximum listed value */

#endif /* !_SYSEXITS_H_ */

```
