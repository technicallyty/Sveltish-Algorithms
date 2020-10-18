<script>
    // ------Svelte Actions--------
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
    // ------End of Svelte Actions--------

    //  ALGORITHM IMPLEMENATION
    const goldsPoreSort = async (origArr) => {
        var arr = origArr;
        var swaps = 1;
        while (swaps != 0) {
            var first = await swapEvens(arr);
            var second = await swapOdds(first.arr);
            arr = second.arr;
            swaps = first.swaps + second.swaps;

            let newish = [...arr];
            proper = [...proper, newish];
            await sleep(speed);
        }

        return arr;
    };

    const swapEvens = async (arr) => {
        var swaps = 0;
        for (var i = 0; i < arr.length; i += 2) {
            if (arr[i] > arr[i + 1]) {
                [arr[i], arr[i + 1]] = [arr[i + 1], arr[i]];
                swaps++;
            }
        }

        return { arr, swaps };
    };

    const swapOdds = async (arr) => {
        var swaps = 0;
        for (var i = 1; i < arr.length - 1; i += 2) {
            if (arr[i] > arr[i + 1]) {
                [arr[i], arr[i + 1]] = [arr[i + 1], arr[i]];
                swaps++;
            }
        }

        return { arr, swaps };
    };

    // End of  ALGORITHM IMPLEMENATION

    //  synchronization utilities
    function sleep(ms) {
        return new Promise((resolve) => setTimeout(resolve, ms));
    }

    async function begin(){
        setTimeout(async () => {

        let x = [...proper[0]];
        await goldsPoreSort(x);
        await sleep(1000);
        threadFinished();
        }, 2000);
    }
    //  end of synchronization utilities

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
