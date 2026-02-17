# Prime-Numbers-In-Python

**Introduction**
This project aims create a function to test if a number is prime in Python.

Questions:
1. Create a function to check if a number is a prime number
2. Counting prime numbers in a range
3. Optimizing the code with math module with sqrt() function

**Tools**
Jupyter Notebook

## **Part 1: Create a function to check if a number is a prime number**

We need to write a function that will return **True** if the provided number is prime number and **False** otherwise. 

The most basic form of this function consists of three parts:
1. Evaluating special cases - in this case, it's 0 and 1
2. Checking the main condition of a number **not** being prime (It's easier to know if a number is not prime, than whether it is)
3. If the above check fails, the number is prime, so return **True**

```python
# Deal with special cases
if n == 0 or n == 1:
    return False
```
We can consider the cases when n is 0 or 1 and return **False** because these numbers are not considered prime numbers. Since we have a return statement here, if it's executed (i.e., if n = 0 or n = 1), Python will ignore the rest of the code of this function. This behavior of the return statement is essential for the rest of the code. 

The reason we are including this check here is, prime numbers are strictly defined for natural numbers greater than 1, which means 0 or 1 should not be considered when checking for prime numbers. But in practical coding, it simplifies the function's future use and accommodates users who may not be aware that prime numbers exclusively apply to numbers greater than 1, as they might attempt to use the function with 0 or 1 regardless.

The next part of the function contains the main piece of code. 

```python
# Loop through all numbers between 2 and n - 1, and check if n is NOT prime
i = 2
while i < n:
    if n % i == 0:
        return False
    i = i + 1
```

The definition of a prime number not divisible by any natural number between 2 and (n - 1), inclusive. This definition leads us to loop through all the numbers between 2 and (n - 1), precisely the purpose of this **while** loop. We can do the same thing with a **for** loop and the **range()** function:

```python
for i in range(2, n):
```

After the loop is set up, we can check whether n is divisible by each number in this loop through the **%** operator. **n % i** would give us the remainder of n divided by i. For n to be divisible by i, the remainder should be 0. 

If that occurs, and n is divisible even once, it means n is not prime. If n is nit prime, we can immediately return False. This will also immediately stop the execution of the function, which is crucial for the next part.

```python
# If the program reached this point, that means the number is prime
return True
```

We immediately stop the function whenever we ensure a number is not a prime number. So, Python will only ever get to the last line in the function of the number itself is prime. That's why we're sure that n is a prime number and can return True.

## **Part 2: Count Prime Numbers in a Range**

Now that we've created the function **is_prime()**, we can use it to find how many prime numbers are there in the range of 100 to 151 inclusive. To do that, we set up a loop that iterates through every single number in the range, tests it for being a prime number, and increases a counter variable if it is.

We have to be mindful of the **range()** function inputs. Consider the range of 100 to 151 inclusive. So, the starting number in range() should be 100. But the end number in range() is not included in the resulting list, instead, the number just before it is considered the last. 

For example, we write range(2, 5), the resulting list will be [2, 3, 4]. 5 is not included, and the list ends with 4. 

So to include 151 in the list, we should write range(100, 152).

```python
count = 0

for i in range(100, 152):
    if is_prime(i):
        count = count + 1

print(count)
```
## **Part 3: Optimizing the code**

When a number is not prime, it implies that it can be expressed as a product of at least two other numbers. For example, consider the number 12. It's not prime because 12 is divisible by 2. You can write it as 2x6. We can also write it as 4x3. Hence, numbers that divide 12 or any other number come in pairs. 

The point is, the numbers we are interested in (those that divide n) come in pairs. If we find one of those numbers, we immediately know the other one as well. 

How does this relate to coding? When testing if the number is prime, we are not interested in how many such pairs there are or precisely the numbers. We're only curious if at least one such number exists. 

One handy mathematical relation - if such a pair exists, then one of the numbers would be smaller than the square root of n, and the other would be bigger than the square root, with only one exception. If both numbers are equal, they're equal to the square root. 

To clarify, let's designate the square root of n as **sqrt(n)**, and according to its definition, **n = sqrt(n) * sqrt()**.
And as previously discussed, product pairs are denoted as **n = a * b** for non-prime numbers. So, **a * b = sqrt(n) * sqrt(n)**. 

a and b are on the opposite sides of **sqrt(n)** (or for **a = b = sqrt(n)** ). You can imagine this as if **sqrt(n)** is the mid-point of all product pairs, where half the factors of n reside to the left of its square root, while the other half are to the right of it.

We return to the same example of 12. The square root of 12 is approximately 3.4. And the product pairs we had were 2x6 and 3x4. 3.4 sits in the middle of both those pairs. 

What does this mean for our code? We only need to search for factors of **n** up to **sqrt(n)** instead of **n -1** because we can find all the available pairs by considering only the numbers up to **sqrt(n)**. If we have not found a number that divides n by the point we reach **sqrt(n)**, we won't find any after that point. After all, if such a number existed, it would have a counterpart smaller than **sqrt(n)**, and we already checked all those. 

That's why the optimization for this function is to not loop from 2 to n-1 but from 2 to the square root of n. 

Finally, in order to include the square root in Python, we need to import the **math** library. Then, we can access the square root through **math.sqrt()**. 

```python
import math

def is_prime_improved(n):
    if n == 0 or n == 1:  
        return False
      
    i = 2
    while i <= math.sqrt(n):
        if n % i == 0:
            return False
        i = i + 1
    return True  
    ```
