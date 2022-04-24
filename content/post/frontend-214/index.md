---
title: '毎日のフロントエンド 214'
date: 2022-04-24T17:21:29+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - Tips
---

## HTML

### `required`属性

- The Boolean required attribute, if present, indicates that the user must specify a value for the input before the owning form can be submitted.
  - If present on any of these input types and elements, the `:required` pseudo class will match.
  - If the attribute is not included, the `:optional` pseudo class will match.

## CSS

### 文本的竖向排版

- `writing-mode`

## Tips

### TypeScript

- Use Generics to dynamically specify the number, and type, of arguments to functions

```ts
export type Event =
  | {
      type: 'LOG_IN';
      payload: {
        userId: string;
      };
    }
  | {
      type: 'SIGN_OUT';
    };

const sendEvent = (eventType: Event['type'], payload?: any) => {};
```

- Create a sendEvent function which only asks for a payload if it's present on the event you're sending.

```ts
export type PEvent =
  | {
      type: 'LOG_IN';
      payload: {
        userId: string;
      };
    }
  | {
      type: 'SIGN_OUT';
    };

const sendEvent = <Type extends PEvent['type']>(
  ...args: Extract<PEvent, { type: Type }> extends { payloads: infer TPayload }
    ? [type: Type, payload: TPayload]
    : [type: Type]
) => {};

sendEvent('SIGN_OUT');
sendEvent('LOG_IN', { userId: '123' });
sendEvent('LOG_IN', {}); // error
sendEvent('LOG_IN'); //error
```

- [Playground Link](https://www.typescriptlang.org/play?jsx=0#code/C4TwDgpgBACgogNwgO2FAvAWAFBSgHygG8c8ypRIAuKAcgBkB5AcQH0BJAOVoG5TyoYAIYgANgHshAExolcAvAFcAzhABO7GVGXA1AS2QBzPvPIBfE+f6E5CyhBq0Ayu2adWjAKoAVXvzwWODgAxuLIOtooUogoaOhQADze4NAQAB7AUcqwMagA2rT2tAC6AHwAFPwAdDVCaobKNHAZakLBwAnwSKgANMQUKTTJkFBmpVDpmchS2UTCYpJaBgBm6lDeMCIS0mZQ-AD8UHn2Qyl989taG1uLxXvyNMeD6ynFOACUGONEgdg4qtNcsBys5XO4vL53iYAdFusCGCwONw+kQoCp1JpHABGABMAGZaKMof8okCQUw2FxaCizFCoAB6ekTNRqcRqEmAuHkxFUumM9Ss9nYGFkgBELjcHh8oveQA)

## Reference

- [haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [HTML attribute: `required` - HTML: HyperText Markup Language | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/required)

- [writing-mode - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/writing-mode)

- [精读《Typescript infer 关键字》 - SegmentFault 思否](https://segmentfault.com/a/1190000040558014)
