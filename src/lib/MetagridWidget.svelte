<script lang="ts">
	import { MetagridApi, credit, MetagridLanguages } from '@metagrid/modern-metagrid-widget';
	import { onMount } from 'svelte';
	// Define props
	export let identifier: string;
	export let provider: string;
	export let language = 'de';
	export let includeDescription = false;
	// local links container
	let links = [];
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

<ul class="metagrid-list">
	{#each links as link}
		<li class="metagrid-item">
			<a
				href={link.url}
				class="metagrid-link"
				target="_blank"
				title={link?.longDescription}
				rel="noreferrer"
			>
				{link.provider}
			</a>
		</li>
	{/each}
</ul>
<!-- eslint-disable vue/no-v-html -->
<div class="metagrid-credit">{@html creditString}</div>
<!--eslint-enable-->
