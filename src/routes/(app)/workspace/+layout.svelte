<script lang="ts">
	import { onMount, getContext } from 'svelte';
	import {
		WEBUI_NAME,
		showSidebar,
		functions,
		user,
		mobile,
		models,
		prompts,
		knowledge,
		tools
	} from '$lib/stores';
	import { page } from '$app/stores';
	import { goto } from '$app/navigation';

	import MenuLines from '$lib/components/icons/MenuLines.svelte';
	import { getModels } from '$lib/apis';
	import { getPrompts } from '$lib/apis/prompts';
	import { getKnowledgeItems } from '$lib/apis/knowledge';
	import { getTools } from '$lib/apis/tools';
	import { getFunctions } from '$lib/apis/functions';

	const i18n = getContext('i18n');

	let loaded = false;

	onMount(async () => {
		if ($user?.role !== 'admin') {
			await goto('/');
		}

		loaded = true;
	});
</script>

<svelte:head>
	<title>
		{$i18n.t('Workspace')} | {$WEBUI_NAME}
	</title>
</svelte:head>

{#if loaded}
	<div
		class=" relative flex flex-col w-full h-screen max-h-[100dvh] {$showSidebar
			? 'md:max-w-[calc(100%-260px)]'
			: ''}"
	>
		<div class="   px-2.5 py-1 backdrop-blur-xl">
			<div class=" flex items-center gap-1">
				<div class="{$showSidebar ? 'md:hidden' : ''} self-center flex flex-none items-center">
					<button
						id="sidebar-toggle-button"
						class="cursor-pointer p-1.5 flex rounded-xl hover:bg-gray-100 dark:hover:bg-gray-850 transition"
						on:click={() => {
							showSidebar.set(!$showSidebar);
						}}
						aria-label="Toggle Sidebar"
					>
						<div class=" m-auto self-center">
							<MenuLines />
						</div>
					</button>
				</div>

				<div class="">
					<div
						class="flex gap-1 scrollbar-none overflow-x-auto w-fit text-center text-sm font-medium rounded-full bg-transparent py-1 touch-auto pointer-events-auto"
					>
						<a
							class="min-w-fit rounded-full p-1.5 {$page.url.pathname.includes('/workspace/models')
								? ''
								: 'text-gray-300 dark:text-gray-600 hover:text-gray-700 dark:hover:text-white'} transition"
							href="/workspace/models">{$i18n.t('Models')}</a
						>

						<a
							class="min-w-fit rounded-full p-1.5 {$page.url.pathname.includes(
								'/workspace/knowledge'
							)
								? ''
								: 'text-gray-300 dark:text-gray-600 hover:text-gray-700 dark:hover:text-white'} transition"
							href="/workspace/knowledge"
						>
							{$i18n.t('Knowledge')}
						</a>

						<a
							class="min-w-fit rounded-full p-1.5 {$page.url.pathname.includes('/workspace/prompts')
								? ''
								: 'text-gray-300 dark:text-gray-600 hover:text-gray-700 dark:hover:text-white'} transition"
							href="/workspace/prompts">{$i18n.t('Prompts')}</a
						>

						<a
							class="min-w-fit rounded-full p-1.5 {$page.url.pathname.includes('/workspace/tools')
								? ''
								: 'text-gray-300 dark:text-gray-600 hover:text-gray-700 dark:hover:text-white'} transition"
							href="/workspace/tools"
						>
							{$i18n.t('Tools')}
						</a>

						<a
							class="min-w-fit rounded-full p-1.5 {$page.url.pathname.includes(
								'/workspace/functions'
							)
								? ''
								: 'text-gray-300 dark:text-gray-600 hover:text-gray-700 dark:hover:text-white'} transition"
							href="/workspace/functions"
						>
							{$i18n.t('Functions')}
						</a>
					</div>
				</div>

				<!-- <div class="flex items-center text-xl font-semibold">{$i18n.t('Workspace')}</div> -->
			</div>
		</div>

		<div class=" -mt-1 pb-1 px-[18px] flex-1 max-h-full overflow-y-auto" id="workspace-container">
			<slot />
		</div>
	</div>
{/if}
