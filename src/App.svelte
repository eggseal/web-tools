<script lang="ts">
    import { onMount } from "svelte";

    type Tool = { name: string; url: string };

    const tools: Tool[] = [
        {
            name: "color-picker",
            url: "https://htmlcolorcodes.com/color-picker/",
        },
        {
            name: "pixel-scaler",
            url: "https://yal.cc/tools/upscale/",
        },
        {
            name: "twitter-video-download",
            url: "https://snapvid.net/en2/twitter-downloader",
        },
        {
            name: "photopea",
            url: "https://www.photopea.com/",
        },
        {
            name: "vectorpea",
            url: "https://www.vectorpea.com/",
        },
        {
            name: "close",
            url: "/",
        },
    ];

    let selectedTool = $state<Tool | null>();

    onMount(() => {
        const update = () =>
            (selectedTool = tools.find(
                (t) => t.name === window.location.hash.replace(/^#\/?/, ""),
            ));

        update();
        window.addEventListener("hashchange", update);
        return () => window.removeEventListener("hashchange", update);
    });
</script>

<main>
    <div class="tool-picker">
        {#each tools as tool}
            <div class="tool-wrapper">
                <a class="list-item" href={`/#/${tool.name}`}>
                    <img
                        class="tool-icon"
                        src={`/icons/${tool.name}.svg`}
                        alt={tool.name.charAt(0)}
                    />
                    <span class="tool-name">
                        {tool.name.replaceAll("-", " ")}
                    </span>
                </a>
                {#if tool.url !== "/"}
                    <a href={tool.url} target="_blank" class="external-item">
                        <img
                            src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fimages.icon-icons.com%2F2551%2FPNG%2F512%2Fexternal_link_icon_152846.png&f=1&nofb=1&ipt=e9f601ab97d63deee0122d641e322f4d5724e3c915db22c421bd2aa940f32d0a&ipo=images"
                            alt=">"
                        />
                    </a>
                {/if}
            </div>
        {/each}
    </div>

    <section class="tool-display">
        {#if !selectedTool || selectedTool.url === "/"}
            <div class="tool-msg">Pick a tool.</div>
        {:else}
            <iframe
                src={selectedTool.url}
                title={selectedTool.name}
                frameborder="0"
            ></iframe>
        {/if}
    </section>
</main>

<style>
    main {
        display: flex;
        padding: 1rem;
        gap: 1rem;
        height: calc(100% - 2rem);
    }

    .tool-picker {
        padding: 0.5rem;
        background-color: #1f1e29;
        border-radius: 8px;
    }

    .tool-wrapper {
        display: flex;
    }

    .list-item {
        flex-grow: 1;
        padding: 0.5rem;
    }

    .list-item,
    .external-item {
        cursor: pointer;
        position: relative;
        display: flex;
        align-items: center;
        gap: 0.5rem;
        text-decoration: none;
        isolation: isolate;
    }
    .list-item::before,
    .external-item::before {
        position: absolute;
        inset: 0.25rem 0;
        content: "";
        border-radius: 8px;
        transition: 150ms ease-in-out;
        z-index: -1;
    }
    .list-item:hover::before,
    .external-item:hover::before {
        background-color: #ffffff10;
    }
    .tool-wrapper:not(:last-child) {
        border-bottom: 1px solid #46455f;
    }
    .tool-icon {
        display: flex;
        align-items: center;
        justify-content: center;
        width: 2rem;
        height: 2rem;
        text-transform: uppercase;
        font-weight: 700;
        filter: brightness(0) invert(1);
    }
    .tool-name,
    .external-item {
        color: #bcbbce;
        text-transform: capitalize;
        text-decoration: none;
    }

    .external-item {
        display: flex;
        position: relative;
        padding: 0 1rem;
    }
    .external-item img {
        width: 1rem;
        filter: brightness(0) invert(1);
    }

    .tool-display {
        flex: 1 1 0;
        display: flex;
        background-color: #0b0b0f;
        border: 1px solid #1f1e29;
        border-radius: 8px;
        overflow: hidden;
    }
    .tool-display:has(.tool-msg) {
        align-items: center;
        justify-content: center;
    }

    .tool-msg {
        color: #46455f;
        font-size: 2rem;
    }
    .tool-display iframe {
        flex: 1 1 0;
    }
</style>
