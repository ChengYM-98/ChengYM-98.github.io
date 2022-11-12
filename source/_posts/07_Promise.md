---
title: 手写Promise
date: 2022-09-18 12:34:08
tags: JS
categories: 前端基础
cover: /img/02.jpg
---
# Promise
直接上手写代码
```js
const PENDDING = 'pendding';
const FULFILED = 'fulfiled';
const REJECTED = 'rejected';

class MyPromise {
    constructor(fn) {
        this.status = PENDDING;
        this.successValue = null;
        this.rejectValue = null;
        this.successCallback = [];
        this.rejectCallback = [];
        try {
            fn(this.resolve.bind(this), this.reject.bind(this));
        } catch (error) {
            this.reject(error);
        }
    }

    resolve(value) {
        if (this.status !== PENDDING) {
            return;
        }
        this.status = FULFILED;
        this.successValue = value;
        while (this.successCallback.length) {
            this.successCallback.shift()();
        }
    }

    reject(value) {
        if (this.status !== PENDDING) {
            return;
        }
        this.status = REJECTED;
        this.rejectValue = value;
        while (this.rejectCallback.length) {
            this.rejectCallback.shift()();
        }
    }

    then(onFulfiled, onRejected) {
        onFulfiled = typeof onFulfiled === 'function' ? onFulfiled : () => { };
        onRejected = typeof onRejected === 'function' ? onRejected : () => { };

        const resolvePromise = (promise2, x, resolve, reject) => {
            if (promise2 === x) {
                return reject(new TypeError('chaining cycle detected for promise '));
            }
            if (x instanceof MyPromise) {
                x.then(resolve, reject);
            } else {
                resolve(x);
            }
        }

        const promise2 = new MyPromise((resolve, reject) => {
            if (this.status === FULFILED) {
                setTimeout(() => {
                    try {
                        const x = onFulfiled(this.successValue);
                        resolvePromise(promise2, x, resolve, reject);
                    } catch (error) {
                        reject(error);
                    }
                })
            } else if (this.status === REJECTED) {
                setTimeout(() => {
                    try {
                        const x = onRejected(this.rejectValue);
                        resolvePromise(promise2, x, resolve, reject);
                    } catch (error) {
                        reject(error);
                    }
                })
            } else if (this.status === PENDDING) {
                //当执行器异步时，需要记录回调函数，等待执行器执行完毕后再执行函数
                this.successCallback.push(() => {
                    setTimeout(() => {
                        try {
                            const x = onFulfiled(this.successValue);
                            resolvePromise(promise2, x, resolve, reject);
                        } catch (error) {
                            reject(error);
                        }
                    })
                });
                this.rejectCallback.push(() => {
                    setTimeout(() => {
                        try {
                            const x = onRejected(this.rejectValue);
                            resolvePromise(promise2, x, resolve, reject);
                        } catch (error) {
                            reject(error);
                        }
                    })
                });
            }
        });
        return promise2;
    }

    catch(callback) {
        this.then(undefined, callback);
    }

    static all(array) {
        const result = [];
        let i = 0;
        return new MyPromise((resolve, reject) => {
            const addData = (key, value) => {
                result[key] = value;
                i++;
                if (i === array.length) {
                    resolve(result);
                }
            };
            array.forEach((item, index) => {
                if (item instanceof MyPromise) {
                    item.then(value => addData(index, value), error => reject(error));
                } else {
                    addData(index, item);
                }
            });
        })
    }

    static race(array) {
        return new MyPromise((resolve, reject) => {
            array.forEach(item => {
                if (item instanceof MyPromise) {
                    item.then(resolve, reject);
                } else {
                    resolve(item);
                }
            })
        })
    }

    static resolve(value) {
        if (value instanceof MyPromise) {
            return value;
        }
        return new MyPromise((resolve, reject) => {
            resolve(value);
        });
    }

    static reject(value) {
        return new MyPromise((resolve, reject) => {
            reject(value);
        })
    }
}
```