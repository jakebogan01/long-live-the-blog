<script>
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();

    export let checked = false;
    export let value = null;

    let proxyChecked = checked;

    function onChange() {
        dispatch("update:checked", proxyChecked);
    }
</script>

<input
    type="checkbox"
    {value}
    bind:checked={proxyChecked}
    on:change={onChange}
    class="h-4 w-4 rounded border-gray-300 text-indigo-600 focus:ring-indigo-600"
/>
