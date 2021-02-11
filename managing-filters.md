# Reactjs: Managing filter

State management is too important when you work with front end development, and with react, this would be no different. And when you are working in a filter for many reasons you should maintain the filter state in URL, when you look at a specific page in one application and you press F5 your application should be able to display the same page with the same results and filter state or tabs opened. And the better way to make this is to persist this information in URL.

When you figure out this is normal you search for an easy way to implement this, but not always it's easy. I want to show you a way to implement that.

## Let see an example of an implementation that shows you how to do this.

For this example, I will use this repo https://github.com/use-form/use-query-filter, let start installing a package to deal with this.

```bash
yarn add @use-form/use-query-filter
```

`use-query-filter` - This package provides a way to managing filter state without effort. `use-query-form` provides a hook called `useQueryFilter` that provide a similar experience that a hook form provides, with it, you can pass an initial state object and receives two functions that change filter state `set, reset`.

`set`: This function can receive a new state or can receive a function that receives the current filter state, then you can update the state by changing just the property you want to change; `set({ property: newValue })` or `set(state=>({...state, property: value }))`.

`reset`: This function will reset the filter state, also it receives a function that receives the current state by parameter, and it will be run when the filter state is reset.

## Show me the code.

In this example, you will build a simple filter with some generics fields.

```jsx 
function Filter() {
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
}
```

At the beginning of the code is set the initial values, nothing complicated here, after having the functions that handle the form submit and reset events `handleSubmit` and `handleReset`.

After you can see the return of the function component, and it used a `FilterProvider` that receives the useQueryFilter hook return. `FilterProvider` is very helpful when you want to build a full page with filter, list, and pagination because the pagination should know and should change the filter state since the `_offset` and `_limit` are pagination properties and these properties should belong the object filter.

Let me show you an example:

```jsx
function AnotherPart() {
  const [filter, { set }] = useFilterContext();

  return (
    <>
      <input
        name="keywords"
        onChange={(e) => set({ keywords: e.target.value })}
      />

      <select onChange={(e) => set({ sort: e.target.value })}>
        <option value="az">A - Z</option>
        <option value="za">Z - A</option>
      </select>
    </>
  );
}
```

If you are interested in what happing with the filter state you can use `useFilterContext` and with this hook is possible to change and receives the current state of the filter.

In a real situation is possible that you use something like that:

```jsx
const initialValues = {
  keywords: '',
  sort: 'az',
};

function Container() {
  const filter = useQueryFilter(initialValues);

  return (
    <FilterContext.Provider value={filter}>
      <Filter />
      <List />
      <Pagination />
    </FilterContext.Provider>
  );
}
```

```jsx
function Filter() {
  const [filter, { set }] = useFilterContext();

  return (
    <>
      <input
        name="keywords"
        onChange={(e) => set({ keywords: e.target.value })}
      />

      <select onChange={(e) => set({ sort: e.target.value })}>
        <option value="az">A - Z</option>
        <option value="za">Z - A</option>
      </select>
    </>
  );
}
```

```jsx
function List() {
  const [filter, { set }] = useFilterContext();

  return <>...</>;
}
```

```jsx
function Pagination() {
  const [filter, { set }] = useFilterContext();

  return <>...</>;
}
```

The last example is just a small view of what happening and how you can organize your code when writing a page with filter and pagination, and one list, feel free to make points about this.

You can see this example running here https://codesandbox.io/s/use-query-filter-iiend
