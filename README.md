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
合并数组
```js
a = [1, 5, 8]
b = [3, 6, 9]
// 1
var c = a.concat(b)
// 2
for (let i in b){
    a.push(b[i])
}
// 3
a.push.apply(a,b)

var d = Array.from (new Set(c))

并发请求控制
```js
async function fetch(urls, limit) {
    const results = [];
    const executing = [];

    for (const url of urls) {
        const promise = fetch(url).then(response => response.json());
        results.push(promise);

        if (limit <= urls.length) {
            const e = promise.finally(() => executing.splice(executing.indexOf(e), 1));
            executing.push(e);
            
            if (executing.length >= limit) {
                await Promise.race(executing);
            }
        }
    }

    return Promise.all(results);
}

// 使用示例
const urls = ['url1', 'url2', 'url3', 'url4'];
fetch(urls, 2).then(results => {
    console.log(results);
});
```
