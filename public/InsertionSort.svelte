<script>
    import {createEventDispatcher, onMount} from 'svelte';
    export let proper = "test";
    export let speed = 1000;

    $: {
        if(proper.length === 1){
            begin();
        }
    }

    const dispatch = createEventDispatcher();
    function threadFinished() {
        dispatch('message',{
            done: 1
        });
    }

    let insertionSort = async (inputArr) => {
        let length = inputArr.length;
        for (let i = 1; i < length; i++) {
            let key = inputArr[i];
            let j = i - 1;
            while (j >= 0 && inputArr[j] > key) {
                inputArr[j + 1] = inputArr[j];
                j = j - 1;
            }
            let newish = [...inputArr];
            proper = [...proper, newish];
            await sleep(speed);
            inputArr[j + 1] = key;
        }
        return inputArr;
    };
    function sleep(ms) {
        return new Promise((resolve) => setTimeout(resolve, ms));
    }

    async function begin(){
        setTimeout(async () => {

        let x = [...proper[0]];
        await insertionSort(x);
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
    row:first-of-type{
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
