# react-addons-perf

## Installation

```
npm install --save-dev react-addons-perf
```

## Injection

```
import Perf from 'react-addons-perf'
```

## Four Main Methods

- Perf.start(): start measuring performance.
- Perf.stop()
- Perf.printExclusive(): prints total rendering time for components.
- Perf.printWasted(): prints wasted renders- we’ll get to this shortly.

# Usage

render time

```
  componentDidUpdate() {
    Perf.stop()
    Perf.printInclusive()
    Perf.printWasted()
  }

  resetMultiplier() {
    Perf.start()
    this.setState({ multiplier: 2 })
  }
```

Perf.start() can't be placed in the `componentWillMount()`

```
  componentWillMount() {
    window.performance.mark('App')
  }

  componentDidMount() {
    console.log(window.performance.now('App'))
  }
```



# Resource

- [Facebook React Perf Doc](https://facebook.github.io/react/docs/perf.html)
- [How to Benchmark React Components](https://medium.com/code-life/how-to-benchmark-react-components-the-quick-and-dirty-guide-f595baf1014c)
- [Performance Engineering with React](http://benchling.engineering/performance-engineering-with-react/)
- [A Deep Dive into React Perf Debugging](http://benchling.engineering/deep-dive-react-perf-debugging/)


