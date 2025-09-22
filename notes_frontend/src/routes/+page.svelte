<script lang="ts">
  import SearchBar from '$lib/components/SearchBar.svelte';
  import NotesGrid from '$lib/components/NotesGrid.svelte';
  import NoteEditorModal from '$lib/components/NoteEditorModal.svelte';
  import { onMount } from 'svelte';
  import { listNotes } from '$lib/api';
  import type { Note } from '$lib/types';
  import { searchQuery } from '$lib/stores';

  let notes: Note[] = [];
  let loading = true;
  let error: string | null = null;

  // local state (avoid $-prefixed variable names which are reserved in Svelte)
  let search = '';

  async function load() {
    loading = true;
    error = null;
    try {
      const q = search;
      notes = await listNotes(q || undefined);
    } catch (e) {
      let msg = 'Failed to load notes.';
      if (e && typeof e === 'object' && 'message' in e) {
        const m = (e as { message?: unknown }).message;
        if (typeof m === 'string') msg = m;
      }
      error = msg;
    } finally {
      loading = false;
    }
  }

  onMount(async () => {
    const unsub = searchQuery.subscribe(async (q) => {
      search = q;
      await load();
    });
    await load();
    return () => unsub();
  });

  function onSaved() {
    load();
  }
  function onChanged() {
    load();
  }
</script>

<svelte:head>
  <title>Ocean Notes</title>
  <meta name="description" content="Create, view, edit and delete your personal notes." />
</svelte:head>

<section class="stack">
  <SearchBar />
  {#if loading}
    <div class="card loading">
      <div class="spinner" aria-hidden="true"></div>
      <div>Loading notes...</div>
    </div>
  {:else if error}
    <div class="card error">
      <strong>Error:</strong> {error}
    </div>
  {:else}
    <NotesGrid {notes} onChanged={onChanged} />
  {/if}
</section>

<NoteEditorModal on:saved={onSaved} />

<style>
  .stack {
    display: grid;
    gap: 16px;
    padding: 16px 0 40px;
  }
  .loading, .error {
    padding: 18px;
    display: flex; align-items: center; gap: 12px;
  }
  .spinner {
    width: 18px; height: 18px; border-radius: 50%;
    border: 2px solid var(--primary-200);
    border-top-color: var(--primary-600);
    animation: spin .8s linear infinite;
  }
  @keyframes spin { to { transform: rotate(360deg); } }
</style>
