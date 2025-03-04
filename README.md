```js
function deepClone(obj) {
    if (obj === null || typeof obj !== 'object') {
        return obj;
    }
    let clone = {}
    for (let key in obj) {
        if (obj.hasOwnProperty(key)) {
            clone[key] = deepClone(obj[key]);
        }
    }

    if (obj instanceof Date) {
        return new Date(obj);
    }
    if (obj instanceof RegExp) {
        return new RegExp(obj);
    }
    if (obj instanceof Map) {
        return new Map([...obj].map(([key, value]) => [deepClone(key), deepClone(value)]));
    }
    if (obj instanceof Set) {
        return new Set([...obj].map(value => deepClone(value)));
    }
    return clone;
}

// 使用示例
const original = {
    name: "Alice",
    age: 30,
    hobbies: ["reading", "gaming"],
    birthDate: new Date(1993, 6, 10),
    address: {
        city: "Wonderland",
        country: "Dreamland"
    }
};

const copied = deepClone(original);
console.log(copied);
```
