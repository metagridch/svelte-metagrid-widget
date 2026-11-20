<script context="module" lang="ts">
	// unique id per instance for aria-describedby
	let instanceCount = 0;
	// screen reader hint for links opening in a new tab (WCAG 3.2.2 / G201)
	const newTabText: Record<string, string> = {
		de: '(öffnet in neuem Tab)',
		fr: '(s’ouvre dans un nouvel onglet)',
		it: '(si apre in una nuova scheda)',
		en: '(opens in new tab)'
	};
</script>

<script lang="ts">
	import { MetagridApi, credit, MetagridLanguages } from '@metagrid/modern-metagrid-widget';
	import { onMount } from 'svelte';
	// Define props
	export let identifier: string;
	export let provider: string;
	export let language = 'de';
	export let includeDescription = false;
	// accessible name of the link list
	export let label = 'Metagrid';
	// local links container
	let links = [];
	const idPrefix = `metagrid-${++instanceCount}`;
	// cast string to MetagridLanguages
	let realLanguage = language as MetagridLanguages;
	onMount(async () => {
		// fetch links from metagrid server
		const api: MetagridApi = MetagridApi.create(provider);
		links = (await api.fetch(identifier, realLanguage, includeDescription)).data;
	});
	// credit text
	let creditString = credit(realLanguage).innerHTML;
</script>

<div class="metagrid" lang={language}>
	{#if links.length}
		<ul class="metagrid-list" aria-label={label}>
			{#each links as link, i}
				<li class="metagrid-item">
					<a
						href={link.url}
						class="metagrid-link"
						target="_blank"
						title={link?.longDescription}
						aria-describedby={link?.longDescription ? `${idPrefix}-${i}` : undefined}
						rel="noreferrer"
					>
						{link.provider}
						<span class="metagrid-sr-only">{newTabText[language] ?? newTabText.en}</span>
					</a>
					{#if link?.longDescription}
						<span id="{idPrefix}-{i}" class="metagrid-sr-only">{link.longDescription}</span>
					{/if}
				</li>
			{/each}
		</ul>
	{/if}
	<!-- eslint-disable vue/no-v-html -->
	<div class="metagrid-credit">{@html creditString}</div>
	<!--eslint-enable-->
</div>

<style>
	/* visually hidden, still read by screen readers */
	.metagrid-sr-only {
		position: absolute;
		width: 1px;
		height: 1px;
		padding: 0;
		margin: -1px;
		overflow: hidden;
		clip: rect(0, 0, 0, 0);
		white-space: nowrap;
		border: 0;
	}
</style>
