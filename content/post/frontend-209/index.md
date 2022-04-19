---
title: '毎日のフロントエンド 209'
date: 2022-04-18T23:37:56+09:00
description: frontend 每日一练
categories:
  - Tips
---

## Tips

### TypeScript

- A runtime check in a function to the tyoe level.

```ts
export const deepEqualCompare = <Arg>(a: Arg, b: Arg): boolean => {
  if (Array.isArray(a) || Array.isArray(b)) {
    throw new Error('You cannot compare two arrays using deepEqualCompare');
  }
  return a === b;
};

deepEqualCompare(1, 1);
deepEqualCompare([], ['a']); // this gonna always be false
```

- Solution

```ts
type CheckForBadArgs<Arg> = Arg extends any[]
  ? 'You cannot compare two arrays using deepEqualCompare'
  : Arg;

export const deepEqualCompare = <Arg>(
  a: CheckForBadArgs<Arg>,
  b: CheckForBadArgs<Arg>
): boolean => {
  if (Array.isArray(a) || Array.isArray(b)) {
    throw new Error('You cannot compare two arrays using deepEqualCompare');
  }
  return a === b;
};

deepEqualCompare(1, 1);
// Argument of type 'never[]' is not assignable to
// parameter of type '"You cannot compare two arrays using deepEqualCompare"
deepEqualCompare([], ['a']);
```

- [Playground Link](https://www.typescriptlang.org/play?jsx=0#code/C4TwDgpgBAwgFhAxgawGIHsBOAhAhgEwEFMBzAZwB5iSA+KAXimqggA9gIA7fMqXTkAG0AugFgAUFCgB+KAHIAmugCuURP07pga9AFswuTNGAB3dH0yZcIXsrIBLTiSj4IEMAFEAjstwAbGD0DIzkJKQAuJlIAbgkJNjAsbUR0TjJtV3dvXwCgw2hGKlIaAAowvkj4JDQsPCJSSmoaABpygCNKhBQMHAJqRuKJAEpItvR0Pwh+BjoAb3L7ADMoEuIrEAA6ezI16xLcIagAHyOo9a2dyz22ocP5ySkoYDhMdBMoTgh3j0ssEsUVGoNFodPp8k8zBZ1rYHE4XG5PD5-IEwSEhrEHgBfcpGYDKTCcPgMeiMNoYzEYiSZRE5FHBCAlACMzSgjPRVIR2WReSMJRELMEclwcmE6KAA)
