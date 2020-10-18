<script>
    //  Svelte Methods -----------------------------------------
    import {createEventDispatcher} from 'svelte';
    export let stringToSort;
    export let speed = 1000;

    $: {
        if(stringToSort.length === 1){
            begin();
        }
    }

    const dispatch = createEventDispatcher();
    function threadFinished() {
        dispatch('message',{
            done: 1
        });
    }
    //  END Svelte Methods -------------------------------------

    // Algorithm -----------------------------------------------
    let insertionSort = async (inputArr) => {
        let length = inputArr.length;
        for (let i = 1; i < length; i++) {
            let key = inputArr[i];
            let j = i - 1;
            while (j >= 0 && inputArr[j] > key) {
                inputArr[j + 1] = inputArr[j];
                j = j - 1;
            }

            //  update the state 
            let newish = [...inputArr];
            stringToSort = [...stringToSort, newish];
            //  wait a bit for the other algorithms
            await sleep(speed);
            inputArr[j + 1] = key;
        }
        let newish = [...inputArr];
        stringToSort = [...stringToSort, newish];
        await sleep(speed);
        return inputArr;
    };
    //  END Algorithm -------------------------------------------

    //  Timer Utility
    function sleep(ms) {
        return new Promise((resolve) => setTimeout(resolve, ms));
    }

    async function begin(){
        setTimeout(async () => {

        let x = [...stringToSort[0]];
        await insertionSort(x);
        await sleep(20000);
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
    row:first-of-type{
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
