# POSIX Shell Literacy
## How to concatenate string variables
```sh
foo="Hello"
foo="${foo} World"
echo "${foo}"
> Hello World
```
In general to concatenate two variables you can just write them one after another:
```sh
a='Hello'
b='World'
c="${a} ${b}"
echo "${c}"
> Hello World
```
credit [here](https://stackoverflow.com/questions/4181703/how-to-concatenate-string-variables-in-bash)
