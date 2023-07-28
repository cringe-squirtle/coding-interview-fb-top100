# Basic Calculator - 224 Hard
 - with parenthesis but only + - no * /
 - can be solved by monotonic stack ( start popping and aggregate expressions only when meets a ) and until meets a (
 - per above, when popping and aggregate expressions, we are evaluating the expression from right-to-left but `-` does not follows the associative property (`(A−B)−C(A-B)-C(A−B)−C` to be equal to `(C−B)−A(C-B)-A(C−B)−A`)
   - one solution is to reverse the string, or iterate the string in reverse order
   - `-` can be used as the sign (negative) -> `A−B−C could be re-written as A+(−B)+(−C)A + (-B) + (-C)A+(−B)+(−C)`.  Thus no need to reverse
# Basic Calculator II - 227 medium
  - without parenthesis
  - with +-*/
  - solutions:
    - brutle force / intuition:
      - use stack
    - without stack
      - record previous number value
      - only add on +-, only update previous number for */
# Basic Calculator III - 772 Hard
  - with parenthesis
  - with +-*/
  - solution:
    - use stack
    - push number as positive and negative into array, so that when popping, just do add
    - when meets a (, also push its previous operator
    - when meets */, push only evaluted number
    - when meets ), start popping and adding until meets an operator which is the indicator of `(` ( since only pushed it when meets a `(` ) 
    - so that `2*(5+5*2)/3+(6/2+8)` is pushed by order 2 * 5 5 (merging 5+5*2) 15 (merging 2*15) 30 (merging 30/3) 10 + 6 (merging 6/2) 3 merging (3+8) 11 -> [10, 11]
    - each expression will be evaluated in the order of */ inside the parenthesis then merge the parenthesis, the */ outside the parenthesis, in the end sum up the stack
    - the goal is to create a array with  positive negative numbers evaluated by */ ()
   

for Basic Calculator II and can be upgraded for Basic Calculator
``` 
class Solution:
    def calculate(self, s: str) -> int:
        def evaluate(operator, x, y = 0):
            if operator == "+":
                return x
            if operator == "-":
                return -x
            if operator == "*":
                return x * y
            return int(x / y)
        
        stack = []
        curr = 0
        previous_operator = "+"
        s += "@"
        
        for c in s:
            if c == " ":
                continue
            if c.isdigit():
                curr = curr * 10 + int(c)
            else:
                if previous_operator in "*/":
                    stack.append(evaluate(previous_operator, stack.pop(), curr))
                else:
                    stack.append(evaluate(previous_operator, curr))
                
                curr = 0
                previous_operator = c

        return sum(stack)
```

for Basic Calculator III
```
class Solution:
    def calculate(self, s: str) -> int:
        def evaluate(x, y, operator):
            if operator == "+":
                return x
            if operator == "-":
                return -x
            if operator == "*":
                return x * y
            return int(x / y)
        
        stack = []
        curr = 0
        previous_operator = "+"
        s += "@"
        
        for c in s:
            if c.isdigit():
                curr = curr * 10 + int(c)
            elif c == "(":
                stack.append(previous_operator)
                previous_operator = "+"
            else:
                if previous_operator in "*/":
                    stack.append(evaluate(stack.pop(), curr, previous_operator))
                else:
                    stack.append(evaluate(curr, 0, previous_operator))
                
                curr = 0
                previous_operator = c
                if c == ")":
                    while type(stack[-1]) == int:
                        curr += stack.pop()
                    previous_operator = stack.pop()

        return sum(stack)
```
