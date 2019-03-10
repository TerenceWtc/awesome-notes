# Lambda

## Quick Start
Create an interface like:
```
public interface MyMath {
    
    int operation(int x, int y);
}
```

Usage:
```
MyMath add = (x, y) -> x + y;
MyMath minus = (x, y) -> x - y;
MyMath multiple = (x, y) -> x * y;
MyMath divide = (x, y) -> x / y;

int addValue = add.operation(1,2);
int minusValue = minus.operation(2,1);
int multipleValue = multiple.operation(2, 3);
int divideValue = divide.operation(4, 2);
```