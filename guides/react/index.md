
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

For a sample in-browser basic react component example see `p1.html` in the [samples folder](/guides/react/samples).

Web component: Connecting react to the outside html world
------------------------------------------------------------
In some tasks you need to write a web component using react. Sample file `p2.html` shows a sample counter component that emits `onchange` event when you press the button three times, and its value is equal to the counter. 

As you can see in the example `p2.html`, our general approach to make a web component is: 

1. mount the react component on a `<span>` host html element
2. pass the host element as the prop `host` to the react component.
3. Use the `props.host` within the react component to manupulate the host element, e.g., emi events or set its value, etc. 

>>this is NOT the standard web component as defiend in [MDN](https://developer.mozilla.org/en-US/docs/Web/Web_Components). But for practical purposes it is pretty enough for us. Please bear in mind that the term "web component" is a general term used for a veriaty of approaches which are not necessarily in line with the official web component as defined in MDN. For example the reactjs [guide](https://reactjs.org/docs/web-components.html) proposes an alternative approach to create web component.   


