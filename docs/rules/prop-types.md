# Prevent missing props validation in a React component definition (prop-types)

PropTypes improve the reusability of your component by validating the received data.

It can warn other developers if they make a mistake while reusing the component with improper data type.

## Rule Details

The following patterns are considered warnings:

```js
var Hello = React.createClass({
  render: function() {
    return <div>Hello {this.props.name}</div>;
  }
});

var Hello = React.createClass({
  propTypes: {
    firstname: React.PropTypes.string.isRequired
  },
  render: function() {
    return <div>Hello {this.props.firstname} {this.props.lastname}</div>; // lastname type is not defined in propTypes
  }
});
```

The following patterns are not considered warnings:

```js
var Hello = React.createClass({
  render: function() {
    return <div>Hello World</div>;
  }
});

var Hello = React.createClass({
  propTypes: {
    name: React.PropTypes.string.isRequired
  },
  render: function() {
    return <div>Hello {this.props.name}</div>;
  }
});

// Referencing an external object disable the rule for the component
var Hello = React.createClass({
  propTypes: myPropTypes,
  render: function() {
    return <div>Hello {this.props.name}</div>;
  }
});
```

## Rule Options

This rule can take one argument to ignore some specific props during validation.

```
...
"prop-types": [<enabled>, { ignore: <ignore> }]
...
```

* `enabled`: for enabling the rule. 0=off, 1=warn, 2=error. Defaults to 0.
* `ignore`: optional array of props name to ignore during validation.

### As for "exceptions"

It would seem that some common properties such as `props.children` or `props.className`
(and alike) need to be treated as exceptions.

As it aptly noticed in
[#7](https://github.com/yannickcr/eslint-plugin-react/issues/7)

> Why should children be an exception?
> Most components don't need `this.props.children`, so that makes it extra important
to document `children` in the propTypes.

Generally, you should use `React.PropTypes.node` for `children`. It accepts
anything that can be rendered: numbers, strings, elements or an array containing
these types.

Since 2.0.0 children is no longer ignored for props validation.
