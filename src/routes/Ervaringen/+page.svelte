<script>
    import { enhance } from '$app/forms'
    import { Button } from '$lib/index.js'
    export let form
    export let data;

    let loading = false

    // Custom enhancement function
    function handleForm(){
        loading = true

        return async ({ result, update }) => {
            // fake 400ms delay voor user feedback
            await setTimeout(() => {
                update()

                loading = false  
            }, 400);
        }
    }
</script>

<section>

    <h1>Ervaringen</h1>
    <h2>Wat vonden anderen van onze service?</h2>
    <p class="p-top">Wij vinden het belangrijk dat wij iedereen zo goed, persoonlijk en efficiënt mogelijk helpen. Lees hieronder hoe andere studenten onze service hebben ervaren!</p>

    <div>
        <!-- Reviews inladen -->
        {#each data.communities as community}
            <article>
                <span class="name">{community.name}</span>
                <p class="ervaring">{community.ervaring}</p>
            </article>
        {/each}
    </div>

    <!-- Enhanced form -->
    <form action="/Ervaringen" method="POST" use:enhance={handleForm}> 
        <h3>Deel jouw ervaring</h3>
        
        {#if form?.error}
            <p class="message fail">{form.message}</p>
        {/if}

        <fieldset>
            <!-- ?? '' means "if form?.name is null or undefined, use the empty string ('')" -->
            <label><span>Naam</span> <input type="text" name="name" minlength="2" required value="{form?.name ?? ''}" placeholder="Jan Jansen"/></label>
        </fieldset>

        <fieldset>
            <label class="label-ervaring"><span>Voer hier je ervaring in</span> <textarea name="ervaring" rows="10" required value="{form?.ervaring ?? ''}"></textarea></label>
        </fieldset>

            <Button buttonText="Versturen" />
            {#if loading }
                <!-- Aninamtie voor custom enhancement -->
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 300 150"><path fill="none" stroke="#3d4666" stroke-width="16" stroke-linecap="round" stroke-dasharray="300 385" stroke-dashoffset="0" d="M275 75c0 31-27 50-50 50-58 0-92-100-150-100-28 0-50 22-50 50s23 50 50 50c58 0 92-100 150-100 24 0 50 19 50 50Z"><animate attributeName="stroke-dashoffset" calcMode="spline" dur="2" values="685;-685" keySplines="0 0 1 1" repeatCount="indefinite"></animate></path></svg>
            {/if}
            
            <!-- Melding voor user feedback -->
            {#if form?.success}
                <p class:active={form?.success} id="feedback">Bedankt voor het delen van jouw ervaring!</p>
            {/if}
    </form>

</section>

<style>
    
    section, form {
        display: flex;
        flex-direction: column;
        padding: 0 2rem;
    }
    
    section{
        align-items: center;
    }
    h1, h2, span{
        font-weight: 600;
    }

    h3{
        padding-bottom: 1rem;
    }

    .p-top{
        text-align: center;
        margin-bottom: 2rem;
    }

    article{
        padding: 1rem;
        background-color: var(--lightmode-accent-color);
        border-radius: 1rem;
        margin-bottom: 1rem;
    }

    fieldset{
        border-style: none;
    }

    label{
        display: flex;
        flex-direction: column;
        width: 20rem;
        padding: 0 1rem 1rem 1rem;
    }
    textarea{
        resize: none;
        overflow-y: scroll;
        max-height: 10rem;
        padding: .25rem;
    }    

    textarea, input {
        padding: .25rem;
    }

    svg{
        height: 2rem;
        width: 2rem;
        position: absolute;
        transform: translateY(13rem) translateX(9.75rem);
    }

    #feedback{
        padding-top: 1rem;
    }
    
    @media  (min-width: 850px) {

        article{
            width: fit-content;
        }

    }
</style>