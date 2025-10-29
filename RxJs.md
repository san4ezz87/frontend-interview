## RxJs

### **Q: While this request is running, ignore any new clicks — they should not trigger new requests until the current one finishes?**

#### Ask about race condition in requsets and in general

[https://stackblitz.com/edit/stackblitz-starters-god9pzbv?file=src%2Fmain.ts](https://stackblitz.com/edit/stackblitz-starters-god9pzbv?file=src%2Fmain.ts)

**Answer:**

`exhaustMap`

---

### **Q: Two widgets need to load data from the same API endpoint. Your task is to ensure the data is fetched with only one request**

[https://stackblitz.com/edit/stackblitz-starters-zqwkbojc?file=src%2Fmain.ts](https://stackblitz.com/edit/stackblitz-starters-zqwkbojc?file=src%2Fmain.ts)

**Answer:**

`shareReplay(1),`

---
