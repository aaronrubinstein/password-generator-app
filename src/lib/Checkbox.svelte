<script>
    export let label;
    export let checked;
</script>

<label>
    <input type="checkbox" bind:checked={checked}>
    {label}
</label>

<style>
    label {
        font-size: 18px;
        color: var(--almost-white);
        cursor: pointer;
        user-select: none;
        display: flex;
        align-items: center;
        gap: 24px;
    }

    input {
        appearance: none;
        color: currentColor;
        width: 20px;
        height: 20px;
        border: 2px solid currentColor;
        cursor: pointer;
    }

    input:checked {
        background-color: var(--neon-green);
        border: 2px solid var(--neon-green);
        background-image: url('images/icon-check.svg');
        background-repeat: no-repeat;
        background-position: 70% 70%;
    }

    @media (max-width: 580px) {
        label {
            font-size: 16px;
            gap: 20px;
        }
    }


</style>