# Unscientific CSS Selector Benchmarking Tests
These tests were all run on [Opera's Dragonfly test suite](http://scope.bitbucket.org/tests/selector-matching-performance/index.html).

Browsers match selectors from right to left. For example, the class `#id div ul li a` will first try to match all `a`'s in the document, then all `li`'s, then all `ul`'s, etc., until it finally finds the unique `#id` and begins rendering.

This has led to a school of thought that claims using class selectors is the fastest best method for CSS performance. My (admittedly unscientific) tests do not confirm that hypothesis.

Admittedly, some of the reasoning behind the "CSS classes only" school of thought are part of a work flow for avoiding clashing styles and decoupling style selectors from JavaScript selectors. These tests indicate to me that the jury is still out on whether or not CSS classes deliver superior performance.

Unfortunately these tests failed completely in Internet Explorer 9 so there is no data available.

## Evironment

Unless noted, all tests were run on my Macbook Pro with a 2.3 GHz Intel Core i5 processor and 8GB 1333 MHz DDR3 RAM. (I don't have specs on the one Windows result.)

## Results

[Full results here](https://docs.google.com/a/umn.edu/spreadsheet/ccc?key=0AvVcfs0W9L-KdHpWeXg1VlJVMVVpMS1GZDhvQmt0TEE&pli=1#gid=0). Run on November 26, 2012.

### Chrome 25.0.1323.1 dev

#### Best Performing Selectors (< 110% of best)

1. class selector with pseudo class not, e.g. `.a:not(a)` (100%)
2. class selector, e.g. `.a` (100%)
3. class selector with attribute selector, e.g. `.a\[b\]` (103%)
4. id selector, e.g. `#a` (105%)

#### Worst 3 Performing Selectors

1. attribute selector with unknown attribute, e.g. `\[a="b"\]` (324%)
2. attribute starts-with selector with known attribute, e.g. `\[class^="a"\]` (314%)
3. attribute selector with unknown attribute without value, e.g. `\[a\]` (313%)

### Opera 11.11

#### Best Performing Selectors (< 110% of best)

1. tag selector, e.g. a (100%)
2. class selector with pseudo class nth-child, e.g. `.a:nth-child(odd)` (100%)
3. no additional style (102%)

#### Worst 3 Performing Selectors

1. attribute selector with unknown attribute, e.g. `\[a="b"\`] (1531%)
2. attribute selector with unknown attribute without value, e.g. `\[a\]` (675%)
3. attribute starts-with selector with known attribute, e.g. `\[class^="a"\]` (380%)

### Firefox 16 (Windows 7)

#### Best Performing Selectors (< 110% of best)

1. class selector with pseudo class nth-child, e.g. `.a:nth-child(odd)` (100%)
2. tag selector, e.g. `a` (102%)
3. id selector, e.g. `#a` (106%)

#### Worst 3 Performing Selectors

1. attribute starts-with selector with known attribute, e.g. `\[class^="a"\]` (1346%)
2. attribute selector with known attribute, e.g. `\[title="a"\]` (1312%)
3. attribute selector with unknown attribute, e.g. `\[a="b"\]` (1306%)

### Firefox 17

#### Best Performing Selectors (< 110% of best)

1. no additional style (100%)
2. class selector, e.g. `.a` (102%)

#### Worst 3 Performing Selectors

1. tag selector, e.g. `a` (400%)
2. id selector, e.g. `#a` (294%)
3. class selector with attribute selector, e.g. `.a\[b\]` (286%)

### Safari 6.02

#### Best Performing Selectors (< 110% of best)

1. no additional style (100%)
2. class selector, e.g. `.a` (101%)
3. class selector with pseudo class nth-child, e.g. `.a:nth-child(odd)` (101%)
4. id selector, e.g. `#a` (103%)

#### Worst 3 Performing Selectors

1. attribute selector with unknown attribute, e.g. `\[a="b"\]` (516%)
2. attribute starts-with selector with known attribute, e.g. `\[class^="a"\]` (494%)
3. attribute selector with known attribute, e.g. `\[title="a"\]` (477%)

##Conclusions

In most cases "attribute selector with unknown attribute, e.g. `\[a="b"\]` and attribute selector with known attribute, e.g. `\[title="a"\]` showed up in the "3 Worst" category. It's safe to say you should avoid those selectors.

It's a three-way tie for fastest performance on:

* class selector, e.g. `.a`
* id selector, e.g. `#a`
* class selector with pseudo class nth-child, e.g. `.a:nth-child(odd)`

That last one is the most surprising and seems to go against conventional wisdom.

I think the best way to look at this is by the *proportion of users* in your audience. Even though the above three selectors offer the best performance across browsers, your users might not be using those.

For example, the `#id` selector (generally considered the fastest) has poor performance in Firefox 17. If you have a lot of Firefox users in your audience, that's a big performance hit.


