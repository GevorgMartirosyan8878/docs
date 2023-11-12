#### componentDidMount

 - When Strict Mode is on, in development React will call componentDidMount, 
then immediately call componentWillUnmount, and then call componentDidMount again. 
This helps you notice if you forgot to implement componentWillUnmount or if its logic doesn’t 
fully “mirror” what componentDidMount does.

 - Although you may call setState immediately in componentDidMount, 
it’s best to avoid that when you can. 
It will trigger an extra rendering, 
but it will happen before the browser updates the screen. 
This guarantees that even though the render will be called twice in this case, 
the user won’t see the intermediate state. 
Use this pattern with caution because it often causes performance issues. 
In most cases, you should be able to assign the initial state in the constructor instead.
It can, however, be necessary for cases like modals and tooltips
when you need to measure a DOM node before rendering something that depends on its size or position.

#### componentDidUpdate
 - componentDidUpdate will not get called if shouldComponentUpdate is defined and returns false

 - The logic inside componentDidUpdate should usually be wrapped in conditions comparing 
this.props with prevProps, and this.state with prevState.
Otherwise, there’s a risk of creating infinite loops.

 - Although you may call setState immediately in componentDidUpdate, 
it’s best to avoid that when you can. 
It will trigger an extra rendering, but it will happen before the browser updates the screen. 
This guarantees that even though the render will be called twice in this case, 
the user won’t see the intermediate state. 
This pattern often causes performance issues,
but it may be necessary for rare cases like modals and tooltips
when you need to measure a DOM node before rendering something that depends on its size or position.

#### getSnapshotBeforeUpdate
 - getSnapshotBeforeUpdate will not get called if shouldComponentUpdate is defined and returns false

#### render

 - render will not get called if shouldComponentUpdate is defined and returns false.

 - When Strict Mode is on, React will call render twice in development and then throw away one of the results. 
This helps you notice the accidental side effects that need to be moved out of the render method.
