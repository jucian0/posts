# Reactjs: Managing filter

State management is too important when we work with front end development, and with react, this would be no different. And when you are working in a filter for many reason you should maintain the filter state in URL, when you look at a specific page in one application and you press F5 your application should be able to display the same page with the same results and filter state or tabs opened. And the better way to make this is to persist this information in URL.

When you figure out this it's normal you find some easy way todo this, but not always it's easy. I want to show you a way to do this that I consider simple.

## Let see an example of an implementation that shows you how to do this.

Form this example I will use thi repo https://github.com/use-form/use-query-filter, let start installing a package to handle with this.

```bash
yarn add @use-form/use-query-filter
```

`use-query-filter` is a package that provides a way to handle more easily filters and manage their state without effort. `use-query-form` provides a hook called `useQueryFilter` that provide a similar experience that a hook form provide, whit it you can pass an initial state object and receives two functions that change filter state `set, reset`.

## Show me the code.

In this example, you will build a simple filter with some generics fields.

```tsx
type Filter = {
  keywords: string;
  type: string;
};

const App: React.FC = () => {
  const [filter, { set, reset }] = useQueryFilter({
    keywords: '',
    type: 'JSX',
  });

  function handleSubmit() {
    console.log('submit', filter);
  }

  function handleReset() {
    reset((e) => console.log('reset', e));
  }

  return (
    <FilterProvider value={[filter, { set, reset }]}>
      <section>
        <form onSubmit={handleSubmit} onReset={handleReset}>
          <div>
            <input
              placeholder="Keywords"
              onChange={(e) => set({ keywords: e.target.value })}
              value={filter.keywords}
            />
          </div>
          <div>
            <select
              placeholder="Type"
              onChange={(e) => set({ type: e.target.value })}
              value={filter.type}
            >
              <option value="JSX">JSX</option>
              <option value="TSX">TSX</option>
              <option value="JS">JS</option>
            </select>
          </div>
          <div>
            <button type="reset">Reset</button>
            <button type="submit">Submit</button>
          </div>
        </form>
      </section>

      <Content />
    </FilterProvider>
  );
};
```
