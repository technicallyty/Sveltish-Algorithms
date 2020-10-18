<script>
    //  Svelte Methods ---------------------------------------
    import { createEventDispatcher } from "svelte";
    export let stringToSort;
    export let speed = 1000;

    $: {
        if(stringToSort.length === 1){
            begin();
        }
    }

    const dispatch = createEventDispatcher();
    function threadFinished() {
        dispatch("message", {
            done: 1,
        });
    }
    //  END Svelte Methods -----------------------------------

    //  Algorithm --------------------------------------------
    const goldsPoreSort = async (origArr) => {
        var arr = origArr;
        var swaps = 1;
        while (swaps != 0) {
            var first = await swapEvens(arr);
            var second = await swapOdds(first.arr);
            arr = second.arr;
            swaps = first.swaps + second.swaps;

            let newish = [...arr];
            stringToSort = [...stringToSort, newish];
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

    // END Algorithm ----------------------------------------

    //  Timer Utility
    function sleep(ms) {
        return new Promise((resolve) => setTimeout(resolve, ms));
    }

    async function begin(){
        setTimeout(async () => {

        let x = [...stringToSort[0]];
        await goldsPoreSort(x);
        await sleep(1500);
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

{#each stringToSort as item}
    <row>
        {#each item as s}
            <p>{s}</p>
        {/each}
    </row>
{/each}
