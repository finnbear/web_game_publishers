<script>
    import publishers from "$lib/publishers.json";
    import { CircleCheck, CircleHelp } from 'lucide-svelte';

    publishers.sort((a, b) => Object.keys(b).length - Object.keys(a).length);

    const FORMATS = ["iframe", "upload", "link"];
    const PLATFORMS = ["pc", "mobile"];
</script>

<div style="padding: 1rem;">
<h1>Webgame Publishers</h1>
<p>This list is sorted by # of known columns. Contribute on <a href="https://github.com/finnbear/web_game_publishers">GitHub</a>!</p>
<table class="table table-bordered table-striped">
    <thead>
        <tr>
            <th rowspan="2">Publisher Sites</th>
            <th colspan={FORMATS.length}>Formats</th>
            <th rowspan="2">Integration</th>
            <th colspan={PLATFORMS.length}>Platforms</th>
            <th rowspan="2">Backlink</th>
            <th rowspan="2">Thumbnails</th>
        </tr>
        <tr>
            {#each FORMATS as format}
                <th>{format}</th>
            {/each}
            {#each PLATFORMS as platform}
                <th>{platform}</th>
            {/each}
        </tr>
    </thead>
    <tbody>
        {#each publishers as publisher}
            <tr>
                <td>
                    <div>
                        {#each publisher.sites as site}
                            <a href={site.url}>{site.name}</a>
                        {/each}
                    </div>
                </td>
                {#if publisher.formats}
                    {#each FORMATS as format}
                        <td style="text-align: center;">
                            {#if publisher.formats.includes(format)}
                                <CircleCheck/>
                            {/if}
                        </td>
                    {/each}
                {:else}
                    <td colspan={FORMATS.length} style="text-align: center;">?</td>
                {/if}
                {#if publisher.integration}
                    <td style="text-align: center;">
                        <div>
                            {#if publisher.integration.mandatory !== undefined}
                                {#if publisher.integration.mandatory}
                                    <span title="Mandatory"><CircleCheck/></span>
                                {:else}
                                    <span title="Optional"><CircleHelp/></span>
                                {/if}
                            {/if}
                            {#if publisher.integration.url}
                                <a href={publisher.integration.url}>
                                    {publisher.integration.type}
                                </a>
                            {:else}
                                {publisher.integration.type}
                            {/if}
                        </div>
                    </td>
                {:else}
                    <td style="text-align: center;">
                        {#if publisher.integration === undefined}
                            ?
                        {/if}
                    </td>
                {/if}
                {#if publisher.platforms}
                    {#each PLATFORMS as type}
                        <td style="text-align: center;">
                            {#if publisher.platforms.filter(p => p.type == type).length > 0}
                                {#if publisher.platforms.filter(p => p.type == type)[0].mandatory}
                                    <span title="Mandatory"><CircleCheck/></span>
                                {:else}
                                    <span title="Optional"><CircleHelp/></span>
                                {/if}
                            {/if}
                        </td>
                    {/each}
                {:else}
                    <td colspan={PLATFORMS.length} style="text-align: center;">?</td>
                {/if}
                {#if publisher.backlink}
                    <td style="text-align: center;">
                        <div>
                            {#if publisher.backlink.mandatory !== undefined}
                                {#if publisher.backlink.mandatory}
                                    <span title="Mandatory"><CircleCheck/></span>
                                {:else}
                                    <span title="Optional"><CircleHelp/></span>
                                {/if}
                            {/if}
                            {#if publisher.backlink.url}
                                <a href={publisher.backlink.url}>
                                    {publisher.backlink.type}
                                </a>
                            {:else}
                                {publisher.backlink.type}
                            {/if}
                        </div>
                    </td>
                {:else}
                    <td style="text-align: center;">
                        {#if publisher.backlink === undefined}
                            ?
                        {/if}
                    </td>
                {/if}
                <td>
                    {#if publisher.thumbnails}
                        {publisher.thumbnails.join(", ")}
                    {:else if publisher.thumbnails === undefined}
                        ?
                    {/if}
                </td>
            </tr>
        {/each}
    </tbody>
</table>
</div>

<style>
    td > div {
        display: flex;
        flex-direction: column;
    }
</style>