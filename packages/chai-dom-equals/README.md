# Testing Chai Dom Equals

> Part of Open Web Component Recommendation [open-wc](https://github.com/open-wc/open-wc/) Recommendation [open-wc](https://github.com/open-wc/open-wc/)

We want to provide a good set of defaults on how to facilitate your web component.

[![CircleCI](https://circleci.com/gh/open-wc/open-wc.svg?style=shield)](https://circleci.com/gh/open-wc/open-wc)
[![BrowserStack Status](https://www.browserstack.com/automate/badge.svg?badge_key=M2UrSFVRang2OWNuZXlWSlhVc3FUVlJtTDkxMnp6eGFDb2pNakl4bGxnbz0tLUE5RjhCU0NUT1ZWa0NuQ3MySFFWWnc9PQ==--86f7fac07cdbd01dd2b26ae84dc6c8ca49e45b50)](https://www.browserstack.com/automate/public-build/M2UrSFVRang2OWNuZXlWSlhVc3FUVlJtTDkxMnp6eGFDb2pNakl4bGxnbz0tLUE5RjhCU0NUT1ZWa0NuQ3MySFFWWnc9PQ==--86f7fac07cdbd01dd2b26ae84dc6c8ca49e45b50)
[![Renovate enabled](https://img.shields.io/badge/renovate-enabled-brightgreen.svg)](https://renovatebot.com/)

Comparing dom literally is usually not what you want when testing components.
Additionally, when using ShadyDOM and/or ShadyCSS there will be additional classes you are not interested in.

::: tip Info
This is part of the default [open-wc](https://open-wc.org/) recommendation
:::

## Manual Setup

Add the following after chai is loaded

```js
import { chai } from '@bundled-es-modules/chai';
import { chaiDomEquals } from '@open-wc/chai-dom-equals';

chai.use(chaiDomEquals);
```

## Test dom of an element
```js
it('has the following dom', async () => {
  const el = await fixture(`<div><!-- comment --><h1>${'Hey'}  </h1>  </div>`);
  expect(el).dom.to.equal('<div><!-- comment --><h1>Hey  </h1>  </div>');
  expect(el).dom.to.semantically.equal('<div><h1>Hey</h1></div>');
});
```

## Test shadow dom of an element
```js
it('has the following shadow dom', async () => {
  const tag = defineCE(class extends HTMLElement {
    constructor() {
      super();
      this.attachShadow({ mode: 'open' });
    }

    connectedCallback() {
      this.shadowRoot.innerHTML = '<p>  shadow content</p> <!-- comment --> <slot></slot>';
    }
  });
  const el = await fixture(`<${tag}></${tag}>`);
  expect(el).shadowDom.to.equal('<p>  shadow content</p> <!-- comment --> <slot></slot>');
  expect(el).shadowDom.to.semantically.equal('<p>shadow content</p><slot>');
});
```
