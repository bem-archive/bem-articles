# The Syntax for `deps.js`

## The complete implimentation of the deps object:

```js
    {
        block : 'bBlock',
        elem  : 'elem',
        mod   : 'modName',
        val   : 'modValue',
        tech  : 'techName',  // технология, для которой собираются зависимости (например, js)
        mustDeps   : [],     // подключатся до блока
        shouldDeps : [],     // порядок подключения не важен (важно лишь подключить)
        noDeps : [],         // можно отменить какую-то зависимость (например, i-bem__dom_init_auto)
    }
```
