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
    function swap(items, leftIndex, rightIndex) {
        var temp = items[leftIndex];
        items[leftIndex] = items[rightIndex];
        items[rightIndex] = temp;
    }
    function partition(items, left, right) {
        var pivot = items[Math.floor((right + left) / 2)], //middle element
            i = left, //left pointer
            j = right; //right pointer
        while (i <= j) {
            while (items[i] < pivot) {
                i++;
            }
            while (items[j] > pivot) {
                j--;
            }
            if (i <= j) {
                swap(items, i, j); //sawpping two elements
                i++;
                j--;
            }
        }
        return i;
    }

    async function quickSort(items, left, right) {
        var index;
        let newish = [...items];
        proper = [...proper, newish];
        await sleep(speed);
        if (items.length > 1) {
            index = partition(items, left, right); //index returned from partition
            if (left < index - 1) {
                //more elements on the left side of the pivot
                await quickSort(items, left, index - 1);
            }
            if (index < right) {
                //more elements on the right side of the pivot
                await quickSort(items, index, right);
            }
        }
        return items;
    }

    function sleep(ms) {
        return new Promise((resolve) => setTimeout(resolve, ms));
    }

    async function begin(){
        setTimeout(async () => {

        let x = [...proper[0]];
        await quickSort(x, 0, x.length-1);
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
