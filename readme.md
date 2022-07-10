# Frontend Master Intermediate React (v4)

## React Hooks

### useState

Make sure that whatever we put in here as the default state is either already
computed or it's a cheap operation (doesn't need a lot of resources).

We can use function inside of `useState` like this:
```javascript
const state = useState(() => document.createElement('div'));
```

`useState()` will only invoke that function once.

### useEffect

What `useEffect()` is doing is basically creating a side effect for our
particular component.

This is basically where we wanna do things that are outside of our normal
render cycle.

It's better to be explicit when we want the effect to run again. For example:
```javascript
function EffectComponent() {
  const [time, setTime] = useState(new Date());

  // Run useEffect every time `time changed.`
  useEffect(() => {
    const timer = setTimeout(() => setTime(new Date()), 100);
    return () => clearTimeout(timer);
  }, [time]);

  // This function at the end of an effect is to clean up the component.
  // This prevent memory leaks.
  return <h1>useEffect Example: {time.toLocaleTimeString()}</h1>;
}
```

> The point of `useEffect()` is to contain side effects.

### useContext

We can think of `useContext()` like a wormhole in our application that we
can dump stuff in one side and it's gonna come out the other side.

It's a very implicit relationship from the parent component to the child
component. Implicit code make the code harder to follow and debug.

### useRef

If we need the most current value across all renders no matter what, all of
the time, we can use `useRef()` for that.

### useReducer

The idea behind `useReducer()` is "let's break this a part and then make it
testable". `useReducer()` is basically `useState()` with extra steps.

### useMemo

`useMemo()` is for when we need to do something and it's computationally
expensive but we don't want to hold up our react app and also we don't have
to do it very often.

### useCallback

`useCallback()` check if the function is the same. For example:
```javascript
const y = function() {}
y === function() {} // false
```

in the example above, they're both empty function, but they're different
empty function.

It's similar to `useMemo()` which give us back the same thing as long as the
dependency doesn't change.

> We might want to use `memo()` function if we want to use `useCallback()`
> function. `memo()` check the props and only re-renders if the props change.

### useLayoutEffect

`useLayoutEffect()` is very similar to `useEffect()` and we should almost
always `useEffect()`.

With `useEffect()`, our effect is scheduled to happen later, we don't have a
guarantee of when that is, that could be immediately, that could be another
millisecond, the point is that it's asynchronous and therefore we can't
reliably say "this is the next thing that's gonna happen".

With `useLayoutEffect()`, we have guarantee that it's synchronous, it's the
next thing that's gonna happen.

### useImperativeHandle

`useImperativeHandle()` is for libraries that are building something on top of
react.

> We're going to use this either if we're an open source maintainer or if
> we're making a design system.

### useDebugValue

`useDebugValue()` is for debugging custom hooks.

> This is typically for library authors or a design system sort of situation.

## Tailwind CSS

With tailwind, we write zero CSS. No more `style.css`, we're not gonna write
any of our CSS rules in there. Instead, we're gonna use a very small utility
classes and we're gonna use that to style our app.

So basically all of our CSS stuff is gonna live inside of our components as
classes.

> Some of the class name will get quite long because sometimes we apply a lot
> of things, but it also allows we to go really fast because we don't have
> to context switch.

Tailwind is a tool to build CSS, so it doesn't come with default CSS styling
and expect the user to style their own application.

## Code Splitting (Lazy load)

We can increase the loading time by not sending the parts the user don't need
yet, that's what we called code splitting.

It's easy to make code splitting a negative thing in our application as well.
For example, if we code split out our modal, it's just adding latency to
something that didn't need to have latency added anyway. We need to be
splitting out at least a hundred kilobytes before code splitting make any
sense. Otherwise, we're just slowing down the user and complicating our app
for no good reason.

We can code splitting with `lazy()`, like this for example:
```javascript
// import() is function for dynamic import. the usual import is called static
// import because it doesn't execute at runtime but build time, meanwhile the
// import() is execute at runtime.
const Details = lazy(() => import('./Details'));
```

And also `Suspense`, which basically if something hit the component inside
`Suspense` and the component is still loading, do something else until the
component finishes loading.

## Server Side Rendering (SSR)

Performance in general is kind of a concern for frontend developers. Frontend
engineering has a peculiar kind of performance engineering. There's an actual
performance, like how fast can our kubernetes cluster munge and spit back out
those request, but there's another kind of performance like how fast can a
human comprehend what we're trying to communicate to them. One ways we can do
that is server side rendering.

Server side rendering basically render our react app preemptively and send
that to the user, so that immediately the user see something.

This is helpful because with server side rendering we're able to load things
in smaller packets upfront so people can see things sooner and then we load
the big javascript payload.

By making that critical time to first meaningful render as short as possible,
then we can get perceived higher performance.

> Web performance is great, but the thing actually matters is what the user
> think.
>
> What we're doing here with server side rendering is, we're trying to fit our
> app the best way to human psychology possible.
>
> But, server side rendering won't always solve the performance problem,
> sometimes it can make things worse. That's why measuring is important.

## Redux

Redux is a data store that lives side by side with react, and every time we
update the data store, the data store informs react that something changed and
update accordingly.

`useReducer()` is just imitating redux. The big value proposition of redux is
that everything we write in redux is extremely testable, it's extremely
modular.

The tradeoff here is that redux can be kind of hard to follow, like it can be
kind of hard to read the code.

The problem that redux can solve for us is make testing state easier. If
we're going from state A to state B inside of react component, we would have
to expose that to make that testable or pull it out to make it testable.
Whereas with redux, it pulls all of the state management out so we can just
test the state management part.

> Redux should not have any side effect, so if we're depend on an order being
> correct, we're not doing redux correctly.

## Extra Notes

If we use code splitting or other performance thing, make sure to measure it,
**make sure that it's actually is a better performance**.

## References

- [Course website](https://btholt.github.io/complete-intro-to-react-v7/lessons/intermediate-react-v4/welcome-to-intermediate-react-v4).
- [Course github repo](https://github.com/btholt/complete-intro-to-react-v7/tree/main/lessons).
