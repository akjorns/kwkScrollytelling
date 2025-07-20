<script>
    import * as Highcharts from "highcharts";
    import "highcharts/modules/exporting";
    import { Chart } from "@highcharts/svelte";
    import Scroller from "../lib/Scroller.svelte";
    import ObservedArticleText from "../lib/ObservedArticleText.svelte";

    const series = [
        {
            name: "Group 1",
            data: [
                [1990, 3],
                [2000, 4],
                [2010, 1],
                [2020, 1],
            ],
            color: "#8427c9",
        },
        {
            name: "Group 2",
            data: [
                [1990, 2],
                [2000, 5],
                [2010, -2],
                [2020, 2],
            ],
            color: "#ff99fc",
        },
        {
            name: "Group 3",
            data: [
                [1990, 4],
                [2000, 3],
                [2010, 0],
                [2020, 3],
            ],
            color: "#4096fa",
        },
    ];

    let chart;
    let thirdSeriesVisible = false;

    let options = {
        chart: {
            type: "spline",
            backgroundColor: "#e3ff00",
            borderColor: "#007052",
            borderWidth: 5,
            borderRadius: 20,
        },
        title: {
            text: "Another Example Chart",
        },
        subtitle: {
            text: "With a subtitle! And styling!",
        },
        series: [series[0], series[1]],
    };

    function toggleThirdSeries() {
        const existingSeries = chart.series.find((s) => s.name === "Group 3");

        if (existingSeries) {
            existingSeries.remove();
            thirdSeriesVisible = false;
        } else {
            chart.addSeries(series[2]);
            thirdSeriesVisible = true;
        }
    }

    // IntersectionObserver options and callback from previous snippet
    const observerOptions = {
        threshold: [0.85, 0.95],
    };

    const callback = (entries) => {
        entries.forEach((entry) => {
            const elem = entry.target;
            if (entry.intersectionRatio >= 0.9) {
                elem.style.backgroundColor = "#FAD9F9";
            } else {
                elem.style.backgroundColor = "#DAA6D3";
            }
        });
    };
</script>

<div>
    <Scroller layout="left">
        {#snippet sticky()}
            <div class="chart">
                <Chart bind:chart {options} highcharts={Highcharts} />
            </div>
            <button on:click={toggleThirdSeries} class="toggle-button">
                {thirdSeriesVisible ? "Remove Group 3" : "Add Group 3"}
            </button>
            <div>
                <p>
                    You can use Svelte to add and remove data from a Highcharts
                    chart.
                </p>
                <p>
                    When you click the button above, a third group is toggled in
                    the chart. Check out the source code to see how it's done.
                </p>
                <p>
                    <strong>
                        🤔 How might you use other HTML elements, like
                        checkboxes or radio buttons, in a similar way to filter
                        data?
                    </strong>
                </p>
            </div>
        {/snippet}

        {#snippet scrolly()}
            <ObservedArticleText {callback} options={observerOptions}>
                You might notice that this basic template doesn't have certain
                features that are common in scrollytelling.
            </ObservedArticleText>

            <ObservedArticleText {callback} options={observerOptions}>
                For example, you might want a component that doesn't feature a
                sticky component at all. Or a component that is solely a sticky
                component.
            </ObservedArticleText>

            <ObservedArticleText {callback} options={observerOptions}>
                You might also want to add more interactivity or gamify parts of
                your scrollytelling piece.
            </ObservedArticleText>

            <ObservedArticleText {callback} options={observerOptions}>
                <strong>
                    It's up to you to research how to create the effects and
                    functionality that you envision!
                </strong>
            </ObservedArticleText>
        {/snippet}
    </Scroller>
</div>

<style>
    .chart {
        width: 90%;
        margin: 0px auto;
    }

    .toggle-button {
        margin: 20px;
        padding: 20px;
        color: #007052;
        background-color: #0bd956;
        border: solid 2px #007052;
        border-radius: 16px;
        font-size: large;
        cursor: pointer;
        transition: all 0.2s ease;
        box-shadow: 0 4px 0 #007052;
    }

    .toggle-button:active {
        transform: translateY(2px);
        box-shadow: 0 2px 0 #007052;
    }
</style>
