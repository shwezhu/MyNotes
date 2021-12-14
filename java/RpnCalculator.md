# RPN
### e.g.
___
```java
import java.util.ArrayList;
import java.util.List;

public class Test {
    public static void main(String[] args)
    {
        List<String> expressionList =new ArrayList<>();
        expressionList.add("2");
        expressionList.add("*");
        expressionList.add("(");
        expressionList.add("2");
        expressionList.add("+");
        expressionList.add("5");
        expressionList.add("/");
        expressionList.add("2.5");
        expressionList.add(")");
        expressionList.add("-");
        expressionList.add("7.50");
        List<String> resultList =       Calculate.transformToRPN(expressionList);
        String result = Calculate.calculateRPN(resultList);
        
        System.out.println(result);	// print 0.5
    }
}
```
### Source code
___
```java
import java.util.ArrayList;
import java.util.List;
import java.util.Stack;
import java.text.DecimalFormat;

public class Calculate {
    public static List<String> transformToRPN(List<String> expression) {
        List<String> list = new ArrayList<>();
        Stack<String> stack = new Stack<>();

        for (String item : expression) {
            // Handling number
            if (isNumber(item)) {
                list.add(item);
                continue;
            }
            // Handling "("
            if (isLeft(item)) {
                stack.push(item);
                continue;
            }
            // Handling ")"
            if (isRight(item)) {
                while (!stack.empty() && !stack.peek().equals("(")) {
                    list.add(stack.pop());
                }
                // pop "(" without adding to list
                if(!stack.empty()) {
                    stack.pop();
                }
                continue;
            }
            // Last case, deals with operators
            if (isOperator(item)) {
                while (!stack.isEmpty()) {
                    if (getPriority(stack.peek()) >= getPriority(item)) {
                        list.add(stack.pop());
                    }
                    else {
                        break;
                    }
                }
                stack.push(item);
            }
        }// end for
        while (!stack.isEmpty()) {
            list.add(stack.pop());
        }
        return list;
    }

    public static String calculateRPN(List<String> expression) {
        Stack<Double> mathStack=new Stack<>();
        double result = 0;

        for (String item : expression) {
            if (isNumber(item)) {
                mathStack.push(Double.valueOf(item));
            }
            if (isOperator(item)) {
                double r = mathStack.pop();
                double l = mathStack.pop();
                switch (item) {
                    case "+":
                        result = l + r;
                        break;
                    case "-":
                        result = l - r;
                        break;
                    case "*":
                        result = l * r;
                        break;
                    case "/":
                        result = l / r;
                        break;
                }

                mathStack.push(result);
            }
        }

        double tmp = mathStack.pop();
        DecimalFormat df = new DecimalFormat( "0.0");
        return df.format(tmp);
    }

    private static boolean isOperator(String operator) {
        return operator.equals("+") || operator.equals("-") || operator.equals("*") || operator.equals("/");
    }

    private static boolean isNumber(String number) {
        // Integer: number.matches("\\d+")
        return number.matches("^-?\\d+(\\.\\d+)?$");
    }

    // private static boolean isInteger(String number) { return number.matches("\\d+"); }

    private static boolean isLeft(String operator) {
        return operator.equals("(");
    }

    private static boolean isRight(String operator) {
        return operator.equals(")");
    }

    private static int getPriority(String operator) {
        switch (operator) {
            case "*":
            case "/":
                return 1;
            case "-":
            case "+":
                return 0;
        }
        return -1;
    }
}
```



