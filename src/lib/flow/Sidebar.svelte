<script lang="ts">
  import { useDnD } from './DnDProvider.svelte';
 
  const type = useDnD();
 
  const onDragStart = (event: DragEvent, nodeType: string, nodeData: object) => {
    if (!event.dataTransfer) return;
    event.dataTransfer.setData('application/node-data', JSON.stringify(nodeData));
    type.current = nodeType;
    event.dataTransfer.effectAllowed = 'move';
  };
</script>
 
<aside>
  <div class="label">You can drag these nodes to the pane below.</div>
  <div class="nodes-container">
    {#each [
      { class: 'input-node', type: 'input', label: 'Input Node',  data: { icon: 'function', title: 'readFile', subtitle: 'api.ts' } },
      { class: 'default-node', type: 'default', label: 'Default Node' ,data: { icon: 'function', title: 'readFile', subtitle: 'api.ts' }},
      { class: 'output-node', type: 'output', label: 'Output Node', data: { icon: 'function', title: 'readFile', subtitle: 'api.ts' }}
    ] as node}
      <div
        class={`${node.class} node`}
        on:dragstart={(event) => onDragStart(event, node.type, node.data)}
        draggable={true}
      >
        {node.label}
      </div>
    {/each}
  </div>
</aside>
 
<style>
  aside {
    width: 100%;
    background: #fff;
    font-size: 12px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
 
  .label {
    margin: 1rem 0;
    font-size: 0.9rem;
  }
 
  .nodes-container {
    display: flex;
    align-items: center;
    justify-content: center;
  }
 
  .node {
    margin: 0.5rem;
    border: 1px solid #111;
    padding: 0.5rem 1rem;
    font-weight: 700;
    border-radius: 5px;
    cursor: grab;
  }
</style>