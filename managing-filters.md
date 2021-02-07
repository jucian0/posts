# Reactjs: Managing filter

State management is too important when we work with front end development, and with react, this would be no different.

But, how we build a powerful filter and manage its state with less effort and maintain the state persisted into page Url?

## Let see an example of an implementation that shows you how to do this.

Form this example I will use thi repo https://github.com/use-form/use-query-filter, let start installing a package to handle with this.

```bash
yarn add @use-form/use-query-filter
```

`use-query-filter` is a package that provide a way to handle more easily with filters and manager their state without effort. `use-query-form` provides a hook called `useQueryFilter` tha provide a similarly experience that a hook form provide, whit it you can pass a initial state object and receives two functions that change filter state `set, reset`, let see a code example:

```typescript
type Filter = {
  keywords: string;
  type: string;
  date: string;
};

const [filter, { set, reset }] = useQueryFilter<Filter>({
  keywords: '',
  date: '',
  type: 'JSX',
});
```
