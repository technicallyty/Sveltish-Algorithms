<script>
    import { createEventDispatcher } from "svelte";
    export let proper = "test";
    export let speed = 1000;

    $: {
        if(proper.length === 1){
            begin();
        }
    }

    const dispatch = createEventDispatcher();
    function threadFinished() {
        dispatch("message", {
            done: 1,
        });
    }
    const mergeSort = async (arr) => {
        if (arr.length <= 1) {
            return arr;
        }

        let mid = Math.floor(arr.length / 2),
            left = await mergeSort(arr.slice(0, mid)),
            right = await mergeSort(arr.slice(mid));

        let q = await merge(left, right);

        return q;
    };

    const merge = async (arr1, arr2) => {
        let sorted = [];

        while (arr1.length && arr2.length) {
            if (arr1[0] < arr2[0]) sorted.push(arr1.shift());
            else sorted.push(arr2.shift());
        }
        let sortz = sorted.concat(arr1.slice().concat(arr2.slice()));
        let newish = [...sortz];
        proper = [...proper, newish];
        await sleep(speed);
        return sortz;
    };

    function sleep(ms) {
        return new Promise((resolve) => setTimeout(resolve, ms));
    }

    async function begin(){
        setTimeout(async () => {

        let x = [...proper[0]];
        await mergeSort(x);
        await sleep(1000);
        threadFinished();
        }, 2000);
    }
</script>

<style>
    row {
        display: flex;
        margin: 0;
        padding: 0;
        flex-direction: row;
        justify-content: center;
        color: black;
    }
    row:first-of-type {
        color: red;
    }
</style>

{#each proper as item}
    <row>
        {#each item as s}
            <p>{s}</p>
        {/each}
    </row>
{/each}
