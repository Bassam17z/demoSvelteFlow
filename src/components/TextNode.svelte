<script lang="ts">
    import {
      type NodeProps,
      Handle,
      Position,
      useHandleConnections,
      useInternalNode,
      type BuiltInNode,
      type Dimensions as OriginalDimensions
    } from '@xyflow/svelte';
  
    import { nodes } from './nodes-and-edges';
  
    type $$Props = NodeProps<BuiltInNode>;
  
    $$restProps;
  
    $: handleConnections = useHandleConnections({
      type: 'target'
    });
  
    $: connectedNode = useInternalNode($handleConnections[0]?.source);
  
    // Extending Dimensions type to allow string indexing
    type Dimensions = OriginalDimensions & { [key: string]: number };
  
    let dimensions: null | Dimensions = null;
  
    $: {
      if ($connectedNode?.measured.width && $connectedNode?.measured.height) {
        dimensions = {
          width: $connectedNode.measured.width,
          height: $connectedNode.measured.height
        };
      } else {
        dimensions = null;
      }
    }
  
    const updateDimension = (attr: string) => (event: Event) => {
      const target = event.target as HTMLInputElement;
      nodes.update((nds) =>
        nds.map((n) => {
          if (n.id === '2-3') {
            return {
              ...n,
              [attr]: parseInt(target.value)
            };
          }
  
          return n;
        })
      );
  
      $nodes = $nodes;
    };
  </script>
  
  <div class="wrapper gradient">
    <div class="inner">
      {#each ['width', 'height'] as attr}
        <label>node {attr}</label>
        <input
          type="number"
          value={dimensions ? String(dimensions[attr]) : "0"}
          on:change={updateDimension(attr)}
          class="nodrag"
          disabled={!dimensions}
        />
      {/each}
  
      {#if dimensions === null}
        no node connected
      {/if}
    </div>
  </div>
  <Handle type="target" position={Position.Top} />
  