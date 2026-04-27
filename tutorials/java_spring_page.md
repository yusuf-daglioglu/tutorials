# JAVA SPRING PAGE

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Spring Page

`Spring`, pageable yapabilmek için `JPA` tarafında destek sunmaktadır. birkaç basit örnek ile ele alalım:

- örnek

```java
import org.springframework.data.repository.PagingAndSortingRepository;

public interface UserRepository extends PagingAndSortingRepository<User, Integer> {

    List<User> findAllByName(String name, Pageable pageable);

    @Query("select c from Users c where c.age = :age")
    List<User> findUsersCustom(@Param("age") String user, Pageable pageable);
}
```

- farklı bir örnek

```java
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Integer>{

     Page<User> findAll(Pageable pageable);
}
```

```java
@Service
public class UserService{

   @Autowired
   UserRepository userRepository;

   public List<User> findAll(PageRequest pageRequest){
         Page<User> usersPage = userRepository.findAll(pageRequest);
         return usersPage.getContent();
   }
}
```

```java
int page = 0; // index/number of page which we want to fetch from DB
int pageSize = 5; // how much elements have the each page
PageRequest pageRequest = new PageRequest(page, pageSize, new Sort(Sort.Direction.DESC, "userId"));
List<User> userList = userService.findAll(pageRequest);
```

- farklı bir örnek

```java
private List<Employee> findAll() {
    List<Employee> allEmployees = new ArrayList<>();

    // First page of employees -- 5000 results per page
    PageRequest pageRequest = PageRequest.of(0, 5000);
    Page<Employee> employeePage = employeeRepository.findAll(pageRequest);
    allEmployees.addAll(employeePage.getContent());

    // All the remaining employees
    // hasNext() is an offline method. it does not ask to DB runtime if another record(Employee) exist. it a simple fixed boolean inside employeePage instance.
    while (employeePage.hasNext()) {
        Page<Employee> nextPageOfEmployees = employeeRepository.findAll(employeePage.nextPageable());
        allEmployees.addAll(nextPageOfEmployees.getContent());

        // update the page reference to the current page
        employeePage = nextPageOfEmployees;
    }

    return allEmployees;
}
```

__org.springframework.data.domain.PageRequest__, __org.springframework.data.domain.Pageable__ arayüzünden türemiştir.

```java
public interface Pageable {

  // index/number of page which we want to fetch from DB
  int getPageNumber();

  // how much elements have the each page
  int getPageSize();

  // sorting parameters
  Sort getSort();

  // others
}
```

__Pageable__ DB'ye yapılacak istek için gerekli meta bilgileri barındırır.

__Page__ and __Slice__ arayüzleri ise DB'den dönen objeleri wrap eder. ikisinin içindeki temel metotlara bakalım:

slice kelime anlamı: dilim

```java
public interface Page<T> extends Slice<T>{

  // total number of pages
  int getTotalPages();

  // total number of items
  long getTotalElements();

  // others
}
```

```java
public interface Slice<T> {

  // current page number
  int getNumber();

  // page size
  int getSize();

  // number of items on the current page
  int getNumberOfElements();

  // list of items on this page (real data objects. example: List<User>)
  List<T> getContent();

  // others
}
```

Eğer bu bilgileri client'a döndürecek isek; o zaman Java main app metodumuzun tepesine __@EnableSpringDataWebSupport__ koymak zorundayız. Fakat spring-boot uygulamalarında SpringDataWebAutoConfiguration olduğu için bu annotation'a ihtiyacımız yok. Eğer bu annotation olmazsa şu tarz hata ile karşılaşabiliriz:

```text
java.lang.NoSuchMethodException: org.springframework.data.domain.Pageable.<init>()
```

Web örneği:

```java
@GetMapping(path = "/users/page")
Page<User> loadUsersPage(Pageable pageable) {
    return userRepository.findAllPage(pageable);
}
```

Spring, otomatik olarak GET isteğindeki query-param'lara size, page, sort gibi parametreleri ekliyor. Sadece sort özelliğini parametre olarak kabul etmek isteyebilirdik:

```java
@GetMapping(path = "/users/sorted")
List<User> loadUsersSorted(Sort sort) {
  return userRepository.findAllSorted(sort);
}
```

Aşağıdaki örnekte Qualifier, query parametrelerimize prefix atabilmemizi sağlar: 

- custom_size
- custom_sort gibi.

```java
@GetMapping(path = "/users/qualifier")
Page<User> loadUsersPageWithQualifier(@Qualifier("custom") Pageable pageable) {
  
  // any logic is here
}
```
