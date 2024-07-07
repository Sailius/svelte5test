<script lang="ts">
    function createTransitionInt(defaultValue: number) {
        let value = $state(defaultValue);

        function reset() {
            value = value === ('' as unknown as number) ? 50 : value;
            const resetInterval = setInterval(
                () => {
                    if (value < defaultValue) {
                        value++;
                    } else if (value > defaultValue) {
                        value--;
                    } else {
                        clearInterval(resetInterval);
                    }
                },
                250 / Math.abs(value - defaultValue)
            );
        }

        function limit() {
            value = value !== Math.floor(value) ? ('' as unknown as number) : value;
            value = value > 100 ? 100 : value;
            value = value < 0 ? 0 : value;
        }

        function increment() {
            value = value === ('' as unknown as number) ? 50 : value;
            value = value < 100 ? (value += 1) : value;
        }

        return {
            get value() {
                return value;
            },
            set value(newValue) {
                value = newValue;
            },
            reset,
            limit,
            increment
        };
    }

    const counter = createTransitionInt(0);
</script>

<h1>Welcome to SvelteKit</h1>
<button onclick={counter.increment}>Increment</button>
<button onclick={counter.reset}>Reset</button>
<input type="range" step={1} min={0} max={100} bind:value={counter.value} oninput={counter.limit} />
<input type="number" bind:value={counter.value} oninput={counter.limit} />
<p>{counter.value}</p>
