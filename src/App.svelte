<script lang="ts">
  import { writable } from 'svelte/store';
  import {
    SvelteFlow,
    Background,
    Controls,
    MiniMap,
    type NodeTypes,
    type EdgeTypes
  } from '@xyflow/svelte';

  import '@xyflow/svelte/dist/style.css';

  import { nodes, edges } from'./components/nodes-and-edges'
  import CustomNode from './components/CustomNode.svelte';
  import AnnotationNode from './components/AnnotationNode.svelte';
  import ButtonEdge from './components/ButtonEdge.svelte';
  import CircleNode from './components/CircleNode.svelte';
  import ResizableNode from './components/ResizableNode.svelte';
  import ToolbarNode from './components/ToolbarNode.svelte';
  import TextNode from './components/TextNode.svelte';

  import './components/overview.css';

  const nodeTypes: NodeTypes = {
    custom: CustomNode,
    annotation: AnnotationNode,
    circle: CircleNode,
    resizer: ResizableNode,
    tools: ToolbarNode,
    textinput: TextNode
  };

  const edgeTypes: EdgeTypes = {
    button: ButtonEdge
  };
</script>

<div style="height:100vh;" class="overview">
  <SvelteFlow {nodes} {edges} {nodeTypes} {edgeTypes} fitView>
    <Background patternColor="#aaa" gap={16} />
    <Controls />
    <MiniMap zoomable pannable height={120} />
  </SvelteFlow>
</div>