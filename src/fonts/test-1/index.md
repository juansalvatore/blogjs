---
title: 'TEST 2'
date: '2019-12-30'
spoiler: My first blog post
---

How do React function components differ from React classes?

For a while, the canonical answer has been that classes provide access to more features (like state). With Hooks, that’s not true anymore.

Maybe you’ve heard one of them is better for performance. Which one? Many of such benchmarks are flawed so I’d be careful drawing conclusions from them. Performance primarily depends on what the code is doing rather than whether you chose a function or a class. In our observation, the performance differences are negligible, though optimization strategies are a bit different.

In either case we don’t recommend rewriting your existing components unless you have other reasons and don’t mind being an early adopter. Hooks are still new (like React was in 2014), and some “best practices” haven’t yet found their way into the tutorials.

So where does that leave us? Are there any fundamental differences between React functions and classes at all? Of course, there are — in the mental model. In this post, I will look at the biggest difference between them. It existed ever since function components were introduced in 2015 but it’s often overlooked:

Function components capture the rendered values.

Let’s unpack what this means.

Note: this post isn’t a value judgement of either classes or functions. I’m only describing the difference between these two programming models in React. For questions about adopting functions more widely, refer to the Hooks FAQ.

Consider this component:

function ProfilePage(props) {
  const showMessage = () => {
    alert('Followed ' + props.user);
  };

  const handleClick = () => {
    setTimeout(showMessage, 3000);
  };

  return (
    <button onClick={handleClick}>Follow</button>
  );
}
It shows a button that simulates a network request with setTimeout and then shows a confirmation alert. For example, if props.user is 'Dan', it will show 'Followed Dan' after three seconds. Simple enough.

(Note it doesn’t matter whether I use arrows or function declarations in the above example. function handleClick() would work exactly the same way.)

How do we write it as a class? A naïve translation might look like this:

class ProfilePage extends React.Component {
  showMessage = () => {
    alert('Followed ' + this.props.user);
  };

  handleClick = () => {
    setTimeout(this.showMessage, 3000);
  };

  render() {
    return <button onClick={this.handleClick}>Follow</button>;
  }
}
It is common to think these two snippets of code are equivalent. People often freely refactor between these patterns without noticing their implications:


