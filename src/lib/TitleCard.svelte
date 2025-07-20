<script>
    let { intro, title, subtitle } = $props();
</script>

<div class="title-card">
    <div class="content">
        {#if intro}
            <p class="intro-sub">{intro}</p>
        {/if}
        <h1>{title}</h1>
        {#if subtitle}
            <p class="intro-sub">{subtitle}</p>
        {/if}
    </div>
</div>

<style>
    .title-card {
        background-color: #034c36;
        height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
        text-align: center;
        padding: 2rem;
        box-sizing: border-box;
        font-family: "Inter", sans-serif;
    }

    .content {
        max-width: 1000px; /* wider box */
        background-color: #DAA6D3;
        padding: 2rem 25rem; /* more horizontal padding */
        border: 3px solid white;
        border-radius: 2rem;
        box-shadow: 16px 16px #FAD9F9;
    }

    /* intro and subtitle with Montserrat font */
    .intro-sub {
        font-family: "Montserrat", sans-serif;
        color: #FAD9F9;
        font-size: 1.4rem;
        margin: 0 0 1rem 0;
        font-weight: 500;
        letter-spacing: 0.05em;
    }

    h1 {
        font-family: "Playfair Display", serif;
        font-size: 4rem;
        margin: 0 0 1rem 0;
        color: white;
        text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.4);
    }

    @media (max-width: 900px) {
        .content {
            max-width: 90vw;
            padding: 2rem;
        }

        h1 {
            font-size: 3rem;
        }

        .intro-sub {
            font-size: 1.2rem;
        }
    }

    @media (max-width: 600px) {
        h1 {
            font-size: 2.4rem;
        }

        .intro-sub {
            font-size: 1rem;
        }
    }
</style>
