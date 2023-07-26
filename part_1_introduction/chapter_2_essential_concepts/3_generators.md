## 3.3 Generators

**Definition:** Generators are a specific type of coroutine. Because they can be paused and resumed, generators allow the production of a sequence of results over time, rather than computing them at once and giving them back all at once.

### Generators in Javascript

```javascript
function* generatorFunction() {
    console.log('This will be executed first.');
    yield 'Hello, ';
    console.log('I will be printed after the pause');
    yield 'World!';
}
const generator = generatorFunction();

console.log(generator.next().value); // This will be executed first. Hello, 
console.log(generator.next().value); // I will be printed after the pause World!
```

### Generators in PHP

```php
function generatorFunction() {
    echo 'This will be executed first.';
    yield 'Hello, ';
    echo 'I will be printed after the pause';
    yield 'World!';
}

$generator = generatorFunction();

echo $generator->current(); // This will be executed first. Hello, 
$generator->next();
echo $generator->current(); // I will be printed after the pause World!
```