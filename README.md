```js
function deepClone(obj) {
    // 检查是否为 null 或基本数据类型
    if (obj === null || typeof obj !== 'object') {
        return obj;
    }
    // 创建一个新的对象或数组
    const clone = Array.isArray(obj) ? [] : Object.create(Object.getPrototypeOf(obj));
    // 递归拷贝属性
    for (const key in obj) {
        if (obj.hasOwnProperty(key)) {
            clone[key] = deepClone(obj[key]);
        }
    }
    // 处理 Date 对象
    if (obj instanceof Date) {
        return new Date(obj);
    }

    // 处理 RegExp 对象
    if (obj instanceof RegExp) {
        return new RegExp(obj);
    }

    // 处理 Map 对象
    if (obj instanceof Map) {
        return new Map([...obj].map(([key, value]) => [deepClone(key), deepClone(value)]));
    }

    // 处理 Set 对象
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
