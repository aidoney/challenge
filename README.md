## Task 2 - complete the caching fetch library

This question remains unresolved, and it is unclear how to address it under certain conditions.

```
 * 1. The application at /appWithSSRData should properly render, with JavaScript disabled, you should see a list of people.
 * 2. You have not changed any code outside of this file to achieve this.
```

In this piece of code

```js
    const renderApp = async (
        loadDataInServer: boolean
    ): Promise<[string, string?]> => {
    // If the App has provided a preLoadServerData, call it, then acquire the cache to send to the browser
    let initialData;
    if (loadDataInServer && typeof App.preLoadServerData === "function") {
        await App.preLoadServerData();
        initialData = serializeCache();
    }

    return [ReactDOMServer.renderToString(<App />), initialData];
    };

    export default renderApp;
```

we're not passing data into <App />, which results in the server being unable to render the correct list of users.

Ideally, it should be something like

```js
ReactDOMServer.renderToString(<App initialData={initialData} />);
```

However, due to the constraints, it is not permitted to modify any files except for cachingFetch.ts."

## Task 1 - configure and document the repository

There are no linters or pre-push, pre-commit hooks currently in place. Implementing these will ensure clean code and a reliable CI/CD pipeline.

The architecture of our React application needs refactoring. Currently, we fetch data in the root <App /> component and partially pass it as a prop to the <Person /> component. Simultaneously, we're retrieving data at both the <Person /> and <Name /> levels. For a simple application, this logic is unnecessarily complex, making unit testing, component reuse, and future modifications more difficult. We should centralize data fetching in the root component and pass the data as props to child components to streamline the architecture.

We validate the data, but we don't use the validation results.

```js
const data = validateData(rawData);
```

The framework folder name does not clearly indicate its contents. The client, server, mock-server folders should be at the root level since they are primary parts of the application.
