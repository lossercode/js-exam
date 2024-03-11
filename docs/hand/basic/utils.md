# 实用方法

## 防抖节流

防抖动是将多次执行变为最后一次执行，节流是将多次执行变成每隔一段时间执行

### 防抖

```javascript
const debounce = (fn, wait) => {
  let timer = null;
  return function (...args) {
    if (timer) {
      clearTimeout(timer);
    }
    timer = setTimeout(() => {
      fn.apply(this, args);
    }, wait);
  };
};
```

### 节流

```javascript
// 第一种方式，使用时间差计算，第一次触发会立即执行，最后一次不会执行
const throttle = (fn, wait) => {
  let prev = new Date();
  return function (...args) {
    const cur = new Date();
    if (cur - prev > wait) {
      fn.apply(this, args);
      prev = cur;
    }
  };
};

// 第二种方式，使用定时器实现，第一次触发会等待 wait 后才执行，最后一次会执行
const throttle = (fn, wait) => {
  let timer = null;
  return function (...args) {
    if (timer) {
      return;
    }
    timer = settimeout(() => {
      fn.apply(this, args);
      timer = null;
    }, wait);
  };
};
```

## 深浅拷贝

### 浅拷贝

1. `json.stringfy()`: 无法实现函数、regExp、Date 等特殊对象的克隆
2. `Object.assign()`: 无法实现多层嵌套的深拷贝
3. `展开运算符`: 无法实现函数、regExp、Date 等特殊对象的克隆

### 深拷贝

```javascript
const deepCopy = (target, map = new WeakMap()) => {
  if (map.has(target)) {
    return map.get(target);
  }
  if (typeof target !== "object" || target === null) {
    return target;
  }

  const constructor = target.constructor;
  const constructorName = constructor.name;

  if (constructorName !== "Array" || constructorName !== "Object") {
    const newValue = new constructor(target);
    map.set(target, newValue);
    return newValue;
  }

  // 只剩下数组和对象
  const newValue = new constructor();
  for (const key in newValue) {
    if (newValue.hasOwnProperty(key)) {
      newValue[key] = deepCopy(newValue[key], map);
    }
  }
  return newValue;
};
```
