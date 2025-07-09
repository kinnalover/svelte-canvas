<script lang="ts">
  import { SvelteFlow, Controls, type Node, type Edge, useSvelteFlow } from '@xyflow/svelte';
 
  import '@xyflow/svelte/dist/style.css';
 
  import { initialNodes, initialEdges } from './nodes-and-edges';
  import TurboNode from './TurboNode.svelte';
  import TurboEdge from './TurboEdge.svelte';
  import { useDnD } from './DnDProvider.svelte';
  import Sidebar from './Sidebar.svelte';
  let nodes = $state.raw<Node[]>(initialNodes);
  let edges = $state.raw<Edge[]>(initialEdges);
 

  
  const nodeTypes = {
    turbo: TurboNode,
  };
 
  const edgeTypes = {
    turbo: TurboEdge,
  };
 
  const defaultEdgeOptions = {
    type: 'turbo',
    markerEnd: 'edge-circle',
  };


  const { screenToFlowPosition } = useSvelteFlow();
 
  const type = useDnD();
 
  const onDragOver = (event: DragEvent) => {
    event.preventDefault();
 
    if (event.dataTransfer) {
      event.dataTransfer.dropEffect = 'move';
    }
  };
 
  const onDrop = (event: DragEvent) => {
    event.preventDefault();
 
    if (!type.current) {
      return;
    }
 
    const position = screenToFlowPosition({
      x: event.clientX,
      y: event.clientY,
    });
 
    const newNode = {
      id: `${Math.random()}`,
      type: type.current,
      position,
      data: { label: `${type.current} node` },
      origin: [0.5, 0.0],
    } satisfies Node;
 
    nodes = [...nodes, newNode];
  };
</script>
 
<main>
<SvelteFlow bind:nodes {nodeTypes} bind:edges {edgeTypes} {defaultEdgeOptions} fitView ondragover={onDragOver} ondrop={onDrop}>
  <Controls showLock={false} />
  
  <svg>
    <defs>
      <linearGradient id="edge-gradient">
        <stop offset="0%" stop-color="#ae53ba" />
        <stop offset="100%" stop-color="#2a8af6" />
      </linearGradient>
      <marker
        id="edge-circle"
        viewBox="-5 -5 10 10"
        refX="0"
        refY="0"
        markerUnits="strokeWidth"
        markerWidth="10"
        markerHeight="10"
        orient="auto"
      >
        <circle stroke="#2a8af6" stroke-opacity="0.75" r="2" cx="0" cy="0" />
      </marker>
    </defs>
  </svg>
</SvelteFlow>
<Sidebar />
</main>
<style>
  main {
    height: 100vh;
    display: flex;
    flex-direction: column-reverse;
  }
</style>