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
                This data was pulled from the Black Wealth Data Center as well as the US Census
            </p>
            <p>
                The percent of people over the age of 25 with a bachelor's degree or higher was obtained
            </p>
            <p>
                It was then calculated how much per region they are contributing to the national average
            </p>
        {/snippet}

        {#snippet scrolly()}
            <ObservedArticleText {callback} options={observerOptions}>
                The Deep South and Southeast, which include many of the most historically marginalized states, are showing proficiency rates below 27%. That means fewer than 1 in 4 to 1 in 3 eighth graders in these regions can read at a level considered “proficient” by national standards.
            </ObservedArticleText>

            <ObservedArticleText {callback} options={observerOptions}>
                Compare that to the Northeast, where 1 in 3 or more students are meeting or exceeding proficiency. The gap is 7 percentage points between the Deep South and Northeast. That might not sound big, but in education data, that’s a huge performance divide affecting millions of kids.
            </ObservedArticleText>

            <ObservedArticleText {callback} options={observerOptions}>
                And the fact that national scores have stagnated—or in some cases declined—only sharpens the urgency.

If you're building a project or story around this, this disparity is a powerful anchor point. It’s not just about test scores—it’s about which children are systematically being left behind and why.
            </ObservedArticleText>

            <ObservedArticleText {callback} options={observerOptions}>
                That means that only about 1 in 3 eighth graders in the best-performing region are reading at a level the National Assessment of Educational Progress (NAEP) defines as "proficient" — which itself is not an elite benchmark, but a solid, grade-level competency.
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
  top: 35vh !important;
  transform: translateY(-35%) !important;
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
