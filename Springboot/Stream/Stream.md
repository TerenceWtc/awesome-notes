# Stream

## Usage

Example:
A book entity:
```
public class Book {

  private int id;

  private String name;
  
  private double price;

  private String author;

  // getter, setter, constructor ...
}
```

Initialize data:
```
List<Book> books = new ArrayList<>();
Book book1 = new Book(1, "Magazine-A", 200.0, "A");
Book book2 = new Book(2, "Magazine-B", 300.0, "B");
Book book3 = new Book(3, "Comic-C", 200.0, "C");
Book book4 = new Book(4, "Comic-A", 250.0, "A");
books.add(book1);
books.add(book2);
books.add(book3);
books.add(book4);
```

### Foreach

Performs the given action for each element of the collection.  
eg:  
  Before Java8, we use:
  ```
  for (Book book : books) {
      System.out.println(book);
  }
  ```

  Now, we can use:
  ```
  // books.forEach(item -> System.out.println(item));
  books.forEach(System.out::println);
  ```

  Console:
  ```
  Book{id=1, name='Magazine-A', price=200.0, author='A'}
  Book{id=2, name='Magazine-B', price=300.0, author='B'}
  Book{id=3, name='Comic-C', price=200.0, author='C'}
  Book{id=4, name='Comic-A', price=2500.0, author='A'}
  ```

### Map

Returns a stream consisting of the results of applying the given function to the elements of this stream.

eg:
  - Square
  ```
  List<Integer> nums = Arrays.asList(1, 2, 3, 4);
  List<Integer> squares = nums.stream()
          .map(n -> n * n)
          .collect(Collectors.toList());
  squares.forEach(System.out::println);
  ```

  Console:
  ```
  1
  4
  9
  16
  ```

  - Field
  ```
  List<String> names = books.stream()
          // .map(n -> n.getName());
          .map(Book::getName)
          .collect(Collectors.toList());
  names.forEach(System.out::println);
  ```

  Console:
  ```
  Magazine-A
  Magazine-B
  Comic-C
  Comic-A
  ```

### Filter

Returns a stream consisting of the elements of this stream that match the given predicate.  
eg:
  ```
  List<Book> result = books.stream()
          .filter(item -> "A".equals(item.getAuthor()))
          .collect(Collectors.toList());
  result.forEach(System.out::println);
  ```

  Console:
  ```
  Book{id=1, name='Magazine-A', price=200.0, author='A'}
  Book{id=4, name='Comic-A', price=2500.0, author='A'}
  ```

### Reduce

Performs a on the elements of this stream.
eg:
  ```
  double sum = books.stream()
          .map(Book::getPrice)
          .reduce(0.0, Double::sum);
  System.out.println(sum);
  ```

  Console:
  ```
  950.0
  ```

## Max / Min

Returns the maximum / minimum element of this stream according to the provided.
eg:
  - Max
  ```
  double max = books.stream()
          .map(Book::getPrice)
          .max(Comparator.naturalOrder())
          .get();
  System.out.println(max);
  ```

  Console:
  ```
  300.0
  ```

  - Min
  ```
  Book book = books.stream()
          .min(Comparator.comparingDouble(Book::getPrice))
          .get();
  System.out.println(book);
  ```

  Console:
  ```
  Book{id=1, name='Magazine-A', price=200.0, author='A'}
  ```