<script>
    import * as Highcharts from "highcharts";
    import "highcharts/modules/exporting";
    import { Chart } from "@highcharts/svelte";
    import Scroller from "../lib/Scroller.svelte";
    import ObservedArticleText from "../lib/ObservedArticleText.svelte";
let options = {
    chart: {
        type: "bar",
        backgroundColor: "#FAD9F9",
    },
    title: {
        text: "Percent of Poverty by Region",
    },
    plotOptions: {
        series: {
            dataLabels: {
                enabled: true,
                format: "{point.y}",
            },
        },
    },
    tooltip: {
        formatter: function () {
            return `<b>${this.point.name}: ${this.point.y}</b>`;
        },
        useHTML: true,
    },
    xAxis: {
        type: "category",
        title: {
            text: "Region",
        },
    },
    yAxis: {
        title: {
            text: "Percent",
        },
    },
    legend: {
        enabled: false,
    },
    series: [
        {
            name: null,
            data: [
                { name: "Deep South", y: 16, color: "#034c36" },
                { name: "Southeast", y: 14, color: "#034c36" },
                { name: "Northeast", y: 10.7, color: "#034c36" },
                { name: "Midwest", y: 11.4, color: "#034c36" },
                { name: "West", y: 11.9, color: "#034c36" },
            ],
        },
    ],
};


    // IntersectionObserver options and callback, from your second snippet
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
    <Scroller layout="right">
        {#snippet sticky()}
            <div class="chart">
                <Chart {options} highcharts={Highcharts} />
            </div>
            <p>
                For the purposes of this analysis, the five regions are defined as: 
            </p>
            <p>
                <strong>Deep South: </strong> Louisiana, Mississippi, Alabama, Georgia, South Carolina
            </p>
            <p>
                <strong>Southeast: </strong> Florida, North Carolina, Tennessee, Arkansas, Kentucky, Virginia, West Virginia
            </p>
            <p>
                <strong>Northeast: </strong> New York, New Jersey, Pennsylvania, Massachusetts, Connecticut, Rhode Island, Vermont, New Hampshire, Maine
            </p>
            <p>
               <strong>Midwest: </strong> Ohio, Indiana, Illinois, Michigan, Wisconsin, Minnesota, Iowa, Missouri, Kansas, Nebraska, South Dakota, North Dakota
            </p>
            <p>
               <strong>West: </strong> Texas, Oklahoma, New Mexico, Colorado, Arizona, Utah, Nevada, California, Oregon, Washington, Alaska, Hawaii, Idaho, Montana, Wyoming
            </p>
        {/snippet}

        {#snippet scrolly()}
            <ObservedArticleText {callback} options={observerOptions}>
                Economic conditions vary widely across the United States, influencing the opportunities available to children in different regions.
            </ObservedArticleText>

            <ObservedArticleText {callback} options={observerOptions}>
                Poverty is much more prevalent in <strong>southern states</strong>. 
                Success isn't defined by financial means, but access to resources is vital for preparing children for the future.
                <br /><br />
                The poverty rate in <strong>Jenny’s</strong> state is 73% higher than in <strong>Grace’s</strong> state.
            </ObservedArticleText>

            <ObservedArticleText {callback} options={observerOptions}>
                <strong>Northern states</strong> typically have much lower poverty rates, which often provides children greater access to opportunities that support their growth and success.
            </ObservedArticleText>

            <ObservedArticleText {callback} options={observerOptions}>
                Addressing these regional disparities is key to ensuring all children have a fair chance to succeed, regardless of where they live.
            </ObservedArticleText>
        {/snippet}
    </Scroller>
</div>

<style>
    .chart {
        width: 90%;
        margin: 0px auto;
    }
    p {
    color: white;
    line-height: 1.2;
    margin-bottom: 0.5rem; /* optional: less vertical space between paragraphs */
}

    strong {
        color: white;
    }

</style>
