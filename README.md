# use-deferred
React hook to handle the deferred promise.

## Install
```sh
npm i use-deferred
```

## Example
```jsx
import useDeferred from 'ues-deferred'

function App(){
    const {execute, resolve, isPending} = useDeferred()
    return (
        <>
            <A execute={execute}/>
            {isPending && <B resolve={resolve} />}
        </>)
}

function A({execute}){

    async function onClick(){
        const result = await execute()
        /****/
    }

    /****/
}

function B({resolve}){

    function onClick(){
        resolve(inputRef.current.value)
    }

    /****/
}
```

## API
### useDeferred(handlers?) : defer
Returns an object to handle the deferred promise.

#### handlers
Handler called when deferred state changes.

- `onExecute(...args)`
- `onComplete()`
- `onResolve(value)`
- `onReject(reason?)`

##### Example
```js
const { execute } = useDeferred({
    onExecute(word1, word2){
        console.log((word1 + ' ' + word2).toUpperCase())
    }
})

execute('hello', 'world'); // => 'HELLO WORLD'
```

### State
Properties for the current state.

- `isBefore`
- `isPending`
- `isResolved`
- `isRejected`
- `isComplete`

##### Example
```js
const { execute, isBefore, isPending } = useDeferred()

console.log(`isBefore: ${isBefore}`)
console.log(`isPending: ${isPending}`)

execute()
// => isBefore: true
// => isPending: false
// => isBefore: false
// => isPending: true
```

### defer.execute(...args)
Execute a deferred promise. If there is an existing deferred promise that is not completed, return it.

### defer.forceExecute(...args)
Reject an existing deferred promise and execute a new deferred promise.

##### Example
```js
import { useDeferred, ForceCancelError } from 'use-deferred'

function App(){
    const { execute, forceExecute } = useDeferred()

    // First run.
    async function onExecClick(){
        try {
            await execute()
        } catch(err) {
            console.log(err.isForceCanceled)
            console.log(err instanceof ForceCancelError)
            console.log(err.toString())
        }
    }

    // Second.
    async function onForceExecClick(){
        await forceExecute()
        // => true
        // => true
        // => 'Cancel for forced new execution.'
    }

    /****/
}
```

### defer.resolve(value)
Resolve the current pending promise.

### defer.reject(reason?)
Reject the current pending promise.

## License
MIT