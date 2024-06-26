# How to run Vizro-AI

This guide offers insights into different ways of running Vizro-AI code, including within a Jupyter Notebook, as a Python script, or through integration into an application.

## Jupyter Notebook
To run Vizro-AI code in a Jupyter Notebook, create a new cell and execute the code below to render the described visualization. You should see the chart as output.

??? note "Note: API key"
    Make sure you have followed the [LLM setup guide](../user-guides/install.md#set-up-access-to-a-large-language-model) and
    your api key is set up in a `.env` file in the same folder as your Notebook file (`.ipynb`).

!!! example "Bar chart"

    === "Code for the cell"
        ```py
        import vizro.plotly.express as px
        from vizro_ai import VizroAI

        vizro_ai = VizroAI()

        df = px.data.gapminder()
        vizro_ai.plot(df, "visualize the life expectancy per continent and color each continent")
        ```
    === "Result"
        [![BarChart]][BarChart]

    [BarChart]: ../../assets/user_guides/bar_chart_gdp_per_continent.png

Note that the chart's appearance may not precisely resemble the one displayed below, as it is generated by a generative AI and can vary.

## Python script
You can use Vizro-AI in any standard development environment by creating a `.py` file and executing the following code. As a result, the rendered chart will display in a browser window.

??? note "Note: API key"

    Make sure you have followed [LLM setup guide](../user-guides/install.md#set-up-access-to-a-large-language-model) and
    your API key is set up in the environment where your `.py` script is running with command as below:

    ```bash
    export OPENAI_API_KEY="your api key"
    ```

!!! example "Line chart"

    === "example.py"
        ```py
        import vizro.plotly.express as px
        from vizro_ai import VizroAI

        vizro_ai = VizroAI()

        df = px.data.gapminder()
        fig = vizro_ai.plot(df, "describe life expectancy per continent over time")
        fig.show()
        ```
    === "Result"
        [![LineChart]][LineChart]

    [LineChart]: ../../assets/user_guides/line_chart_life_expect.png

## Application integration

You may prefer to integrate Vizro-AI into an application with a UI that users use to input prompts using a text field.

There are two ways to integrate Vizro-AI into an application, directly and by accessing the chart code behind a `fig` object.

1. Vizro-AI's `plot` method returns a `plotly.graph_objects` object (`fig`) that can be used directly within a `Vizro` dashboard.

    !!! example "Direct application integration"

        === "app.py"
            ```py
            import vizro.plotly.express as px
            from vizro_ai import VizroAI

            vizro_ai = VizroAI()

            df = px.data.gapminder()
            fig = vizro_ai.plot(df, "describe life expectancy per continent over time")
            ```


2. Vizro-AI's `_get_chart_code` method returns a string of Python code that manipulates the data and creates the visualization. Vizro-AI validates the code to ensure that it is executable and can be integrated.

    !!! example "Application integration via chart code"

        === "app.py"
            ```py
            import vizro.plotly.express as px
            from vizro_ai import VizroAI

            vizro_ai = VizroAI()

            df = px.data.gapminder()
            code_string = vizro_ai._get_chart_code(df, "describe life expectancy per continent over time")
            ```
        === "code_string"
            [![ResultCode]][ResultCode]

        [ResultCode]: ../../assets/user_guides/code_string_app_integration.png

    The returned `code_string` can be used to dynamically render charts within your application. You may have the option to encapsulate the chart within a `fig` object or convert the figure into a JSON string for further integration.

    To use the insights or code explanation, you can use `vizro_ai._run_plot_tasks(df, ..., explain=True)`, which returns a dictionary containing the code explanation and chart insights alongside the code.

### How to use `max_debug_retry` parameter in plot function
- Default Value: 3
- Type: int
- Brief: By default, the `max_debug_retry` is set to 3, the function will try to debug errors up to three times.
If the errors are not resolved after the maximum number of retries, the function will stop further debugging retries.
For example, if you would like adjust to 5 retries, you can set `max_debug_retry = 5` in the plot function:

```py
vizro_ai.plot(df = df, user_input = "your user input", max_debug_retry= 5)
```
