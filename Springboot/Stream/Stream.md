# Stream

## Usage

Example:
```
public class Book {

  private int id;

  private String name;
  
  private double price;

  private String author;

  // getter, setter, constructor ...
}
```
Initialize:
```
List<Book> books = new ArrayList<>();
Book book1 = new Book(1, "Magazine-A", 200.0, "A");
Book book2 = new Book(2, "Magazine-B", 300.0, "B");
Book book3 = new Book(3, "Comic-C", 200.0, "C");
Book book4 = new Book(4, "Comic-A", 2500.0, "A");
books.add(book1);
books.add(book2);
books.add(book3);
books.add(book4);
```

### Map
Mapping each element of the input stream to another element of the output stream.

eg:
- Square
  ```
  List<Integer> nums = Arrays.asList(1, 2, 3, 4);
  List<Integer> squares = nums.stream()
          .map(n -> n * n)
          .collect(Collectors.toList());
  ```
  The  `squares` should be a list of (1, 4, 9, 16).

- Field
  ```
  List<String> names = books.stream()
          // .map(n -> n.getName());
          .map(Book::getName)
          .collect(Collectors.toList());
  ```
  The `names` should be a list of ("Magazine-A", "Magazine-B", "Comic-C", "Comic-A").

### Filter
Only those meeting the filter will be left.
