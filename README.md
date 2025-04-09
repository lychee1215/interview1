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


并发请求控制
```js
async function fetchWithLimit(urls, limit) {
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
fetchWithLimit(urls, 2).then(results => {
    console.log(results);
});

import pLimit from 'p-limit';

const limit = pLimit(2); // 设置并发限制为2

const urls = ['url1', 'url2', 'url3', 'url4'];
const promises = urls.map(url => limit(() => fetch(url).then(res => res.json())));

Promise.all(promises).then(results => {
    console.log(results);
});

```
```js
const FeedbackSystem = () => {

  const subjects = ["Readability", "Performance", "Security", "Documentation", "Testing"];
  const [votes, setVotes] = React.useState(subjects.map((s)=>({subject: s, upvotes: 0, downvotes: 0})));

  const handleUpVote = (index) =>{
    setVotes(prevState=>{
      const newData = [...prevState];
      newData[index].upvotes +=1;
      return newData
    })
  }

  const handleDownVote = (index) =>{
    setVotes(prevState=>{
      const newData = [...prevState];
      newData[index].downvotes +=1;
      return newData
    })
  }
  return (
    <div className="my-0 mx-auto text-center w-mx-1200">
      <div className="flex wrap justify-content-center mt-30 gap-30">
        {subjects.map((subject,index) =>(
          <div key={subject} className="pa-10 w-300 card">
          <h2>{subject}</h2>
          <div className="flex my-30 mx-0 justify-content-around">
            <button onClick={()=> handleUpVote(index)} className="py-10 px-15" data-testid={`upvote-btn-0-${subject}`}>
              👍 Upvote
            </button>
            <button onClick={()=> handleDownVote(index)} className="py-10 px-15 danger" data-testid={`downvote-btn-0-${subject}`}>
              👎 Downvote
            </button>
          </div>
          <p className="my-10 mx-0" data-testid={`upvote-count-0-${subject}`}>
            Upvotes: <strong>{votes[index].upvotes}</strong>
          </p>
          <p className="my-10 mx-0" data-testid={`downvote-count-0-${subject}`}>
            Downvotes: <strong>{votes[index].downvotes}</strong>
          </p>
        </div>
        ))}
      </div>
    </div>
  );
};

export default FeedbackSystem;
```
When a react component is re-rendered, which methods are called, and in what order?

getDerivedStateFromProps()
shouldComponentUpdate()
render()
getSnapshotBeforeUpdate()
componentDidUpdate()

```js
import React from 'react';

const AutocorrectTextarea = () => {
  const [text, setText] = React.useState('')
  const corrections = {
  'realy': 'really',
  'wierd': 'weird',
  };

  const autocorrect = (text) => {
    const words = text.split(' ')
    const correctedWords = words.map((word) =>{
      return corrections[word] || word
    })
    return correctedWords.join(' ')

  }

  const handleAutoCorrect = (event) =>{
    const inputText = event.target.value
    const corrected = autocorrect(inputText)
    setText(corrected)
  }


    return (
      <div className="text-center">
        <textarea value ={text} onChange={handleAutoCorrect} data-testid="textarea" rows={10} cols={80} className="card" />
      </div>
    );
  
}

export default AutocorrectTextarea;

```
JS questions
- A callback is a function that will be executed after another function gets executed.
- Constructor functions are used to create objects in JS


Data structure questions
- A linked list is a data structure that is used to store multiple items. The items are linked together using the key.
- A BST is a data structure that maintains a sorted order of elements. That all values of left children are less than that of root and right children are larger than the root. It is kind of similar to binary search. The average time complexity to search and insert is O(logn)
- hashmap: key and value pair. O(1) to find if you have the key
- Inorder traverse:
```
var inorderTraversal = function(root) {
    let ans = []
    const inOrder = (node) => {
        if(!node){
            return
        }
        inOrder(node.left)
        ans.push(node.val)
        inOrder(node.right)
    }
    inOrder(root)
    return ans

};
```
