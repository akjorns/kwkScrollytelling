<script>
    import * as Highcharts from "highcharts";
    import "highcharts/modules/exporting";
    import { Chart } from "@highcharts/svelte";
    import Scroller from "../lib/Scroller.svelte";
    import ObservedArticleText from "../lib/ObservedArticleText.svelte";

    let options = {
        chart: {
            type: "pie",
            backgroundColor: "#FAD9F9",
        },
        title: {
            text: "Regional Contribution to National Bachelor's Degree Holders",
        },
        plotOptions: {
            pie: {
                allowPointSelect: true,
                dataLabels: [
                    {
                        enabled: true,
                        distance: 20,
                    },
                    {
                        enabled: true,
                        distance: -40,
                        format: "{point.percentage:.1f}%",
                        style: {
                            fontSize: "1.2em",
                            textOutline: "none",
                        },
                        filter: {
                            operator: ">",
                            property: "percentage",
                            value: 10,
                        },
                    },
                ],
            },
        },
        series: [
            {
                name: "Regional Contribution to National Bachelor's Degree Holders",
                data: [
                    { name: "Deep South", y: 17.4, color: "#034c36" },
                    { name: "Southeast", y: 18.6, color: "#0a6a4f" },
                    { name: "Northeast", y: 23.9, color: "#023823" },
                    { name: "Midwest", y: 19.8, color: "#016c41" },
                    { name: "West", y: 20.3, color: "#356c59" }
                ],
            },
        ],
    };

    const observerOptions = {
        threshold: [0.85, 0.95],
    };

    const callback = (entries) => {
        entries.forEach((entry) => {
            const elem = entry.target;
            elem.style.backgroundColor = entry.intersectionRatio >= 0.9 ? "#FAD9F9" : "#DAA6D3";
        });
    };
</script>

<div>
    <Scroller layout="left">
        {#snippet sticky()}
            <div class="chart">
                <Chart {options} highcharts={Highcharts} />
            </div>
            <p>
                Here's an example chart using
                <a href="https://www.highcharts.com/">Highcharts</a>!
            </p>
            <p>
                📈 <strong>Highcharts</strong> is a super-flexible library for
                creating all kinds of charts. See demos of different chart types
                <a href="https://www.highcharts.com/demo">here</a>.
            </p>
            <p>
                Since we're using Highcharts through Svelte, the syntax is a
                little different from what you might see in the demos. But all
                of Highcharts' functionality is available through the Highcharts
                for Svelte package.
            </p>
            <p>
                The configuration is done through the
                <code>options</code> json object passed to the chart.
            </p>
            <p>
                Use the
                <a href="https://api.highcharts.com/highcharts/">API reference</a>
                to understand what each element in the <code>options</code> object does.
            </p>
        {/snippet}

        {#snippet scrolly()}
            <ObservedArticleText {callback} options={observerOptions}>
                <strong>Welcome to the KWK Data Scrollytelling Template!</strong>
            </ObservedArticleText>

            <ObservedArticleText {callback} options={observerOptions}>
                This is a <strong>basic example</strong> of how you might create
                a scrollytelling piece using Svelte and Highcharts.
            </ObservedArticleText>

            <ObservedArticleText {callback} options={observerOptions}>
                You can use this template as a <strong>starting point</strong> for your project.
            </ObservedArticleText>

            <ObservedArticleText {callback} options={observerOptions}>
                <strong>
                    If you use this template, be sure to modify it and make it your own!
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

    :global(.wrapper.left) :global(.sticky) {
  top: 40vh !important;
  transform: translateY(-35%) !important;
}

</style>
