
React general guide
======================================

In some of the task you need to use react, particularly you'll need to create some react components. Here are the standards and conventions we follow in our project.

Use preact
-----------------
For now we use [preact](https://preactjs.com/) which is the same as reactjs, but is smaller and faster. You can still follow the official react [guide](https://reactjs.org/docs/getting-started.html) though.

Use template lit instead of jsx
------------------------------------
It is common to use JSX with react, but modern javascript offers [template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals), and now it is possible use native js template instead of jsx. To do so you'll need to use the [htm tool](https://github.com/developit/htm)(in this link there are examplses provided). 

The bottomline is you can use react directly in the browser `<script>` without having to use webpack and babel to compile your jsx. In our projects we adopt this new approach over JSX. Note that for the production it is possible to compile the htm template literals into functions calls (similar to jsx), and so removing the overhead of string parsing.   

For a sample in-browser basic react component example see [p1.html](/guides/react/samples/p1.html) in the [samples folder](/guides/react/samples).

p1.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<h1> Using preact in browser (good for quick testing!) </h1>
<p style="margin-bottom: 50px"> Hopefully your browser supports latest javascript syntax!</p>

<section>
    <h2>Below is the element hosting the react component </h2>
    <span id="host1"></span>
</section>

<script type="module">
    import { h, render } from 'https://unpkg.com/preact@latest?module';
    import { useState, useEffect } from 'https://unpkg.com/preact@latest/hooks/dist/hooks.module.js?module';
    import htm from 'https://unpkg.com/htm?module';

    // Initialize htm with Preact
    const html = htm.bind(h);

    // your react component
    function Something(props) {

        // counter state
        const [counter, setCounter] = useState(1);

        const handleClick = () => setCounter(counter+1);

        return html`
        <h2>Hello ${props.name}!</h2>
        <button onClick=${handleClick}> Count! (${counter})</button>
        `;
    }

    // rendering the component on the host element
    var host = document.getElementById("host1")
    render(html`<${Something} name="World" />`, host);
</script>

</body>
</html>
```

Web component: Connecting react to the outside html world
------------------------------------------------------------
In some tasks you need to write a web component using react. Sample file [p2.html](/guides/react/samples/p2.html) shows a sample counter component that emits `onchange` event when you press the button three times, and its value is equal to the counter. 

p2.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<h1> A react web component that triggers on click event to the outside worls </h1>
<p style="margin-bottom: 50px"> Hopefully your browser supports latest javascript syntax!</p>

<section>
    <h2>Below is the element hosting the react component </h2>
    <span id="host1" onchange="done(this)"></span>
</section>

<script>
    // on click handler for the html element hosting teh react component

    function done(el) {
        alert(`Ok! Done! you clicked 3 times! The host value is ${el.value}`);

    }

</script>

<!-- module script for teh react part -->
<script type="module">

    import { h, render } from 'https://unpkg.com/preact@latest?module';
    import { useState, useEffect } from 'https://unpkg.com/preact@latest/hooks/dist/hooks.module.js?module';
    import htm from 'https://unpkg.com/htm?module';

    // Initialize htm with Preact
    const html = htm.bind(h);

    // your react component
    function Something(props) {

        // counter state
        const [counter, setCounter] = useState(1);

        // set element value to 1;
        props.host.value = 1;

        const handleClick = () => {
            props.host.value = counter;
            if (counter <= 2 )
                setCounter(counter+1);
            else {
                setCounter(0);
                props.host.dispatchEvent(new Event("change"));
            }
        }

        return html`
        <h2>Hello ${props.name}!</h2>
        <p> After pressing the button 3 times an on-change event will be sent to the html world!</p>
        <button onClick=${handleClick}> Count! (${counter})</button>
        `;
    }

    // rendering the component on the host element. Note we passed a reference to the host element as a prop
    var host = document.getElementById("host1");
    render(html`<${Something} name="Payam" host=${host} />`, host);
</script>

</body>
</html>
```

As you can see in the example `p2.html`, our general approach to make a web component is: 

1. mount the react component on a `<span>` host html element
2. pass the host element as the prop `host` to the react component.
3. Use the `props.host` within the react component to manupulate the host element, e.g., emi events or set its value, etc. 

>>this is NOT the standard web component as defiend in [MDN](https://developer.mozilla.org/en-US/docs/Web/Web_Components). But for practical purposes it is pretty enough for us. Please bear in mind that the term "web component" is a general term used for a veriaty of approaches which are not necessarily in line with the official web component as defined in MDN. For example the reactjs [guide](https://reactjs.org/docs/web-components.html) proposes an alternative approach to create web component.   


