---
title: Java 枚举
date: 2019-05-13 19:37:16
categories:
- Java
---
枚举类型（enum type）的指由一组固定的常量组成合法值的类型。

下面看一个简单的例子，使用枚举类型来定义数学的四大基本操作（加减乘除）。
```Java
public enum Operation {
    PLUS, SUB, MULTI, DIVIDE;

    public Double operator(Double a, Double b) {
        switch (this) {
            case PLUS:
                return a + b;
            case MULTI:
                return a * b;
            case DIVIDE:
                return a / b;
            case SUB:
                return a - b;
        }
        throw new RuntimeException("Invalid operator");
    }
}
```
这个代码已经满足了我们的基本的需求，但是它很脆弱。如果我们添加了新的枚举常量，却没有给switch添加相应的条件，会运行失败。
所以我们可以在枚举类中声明一个抽象的operator方法。在每次定义一个新的枚举的时候，实现operator方法。
```Java
public enum Operation {
    PLUS {
        @Override
        public Double operator(Double a, Double b) {
            return a + b;
        }
    }, SUB {
        @Override
        public Double operator(Double a, Double b) {
            return a - b;
        }
    }, MULTI {
        @Override
        public Double operator(Double a, Double b) {
            return a * b;
        }
    }, DIVIDE {
        @Override
        public Double operator(Double a, Double b) {
            return a / b;
        }
    };

    public abstract Double operator(Double a, Double b);
}
```
但是采用匿名类看上去很繁琐，但是他的核心只有一个return语句而已，所以可以考虑用lambda去简化代码。
```Java
import java.util.function.DoubleBinaryOperator;

public enum OperationLambda {
    PLUS((x, y) -> x + y),
    SUB((x, y) -> x - y),
    MULTI((x, y) -> x * y),
    DIVIDE((x, y) -> x / y),
    ;
    // 代表枚举常量的行为
    private final DoubleBinaryOperator op;

    OperationLambda(DoubleBinaryOperator op) {
        this.op = op;
    }

    public Double operator(Double x, Double y) {
        return op.applyAsDouble(x, y);
    }
}
```