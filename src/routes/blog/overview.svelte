<script context="module">
    export async function preload({parans}, {token}){
        if (!token){
            this.redirect(302, '/login')
        }
    }
</script>


<script>
    import {post} from "utils.js";
    import {getTransitionDurationFromElement} from "bootstrap/js/src/util";
    async function getMyData(){
        return await post('auth/getMyData').then(r=> {
            r.funds = r.accounts.reduce(
                (funds, account) => funds + account.balance,
                0
            )
            return r
        })
    }
    async function getTransitions(){
        return await post('auth/getTransitions')
    }
</script>
{#if process.browser}
{#await getMyData()}
    Loading....
    {:then ny}
    <section>
        <p>{ny.name}</p>
    </section>
    <section>
        <p>{ny.funds}</p>
    </section>
    <section>
        <ul>
            {#each ny.account as account}
            	<li>{account.number} ({account.name})</li>
            {/each}
        </ul>
    </section>
    <section>
        {#await getTransitions()}
            Loading....
        {:then transitions}
            <table>
                {#each transitions as tarnsaction}
            <tr>
            <td>{transitions.status}</td>
            <td>{transitions.userId}</td>
            <td>{transitions.amount}</td>
            <td>{transitions.currency}</td>
            <td>{transitions.AcountFrom}</td>
            <td>{transitions.accountTo}</td>
            <td>{transitions.explantion}</td>
            <td>{transitions.StatusDetail}</td>
            <td>{transitions.senderName}</td>
            <td>{transitions.createdAt}</td>
            </tr>
                {/each}
            </table>
            "_v": 0,
            "id":
            <pre>{JSON.stringify(ny, null, 4)}</pre>

            {/await}
    </section>


    {/await}
    {/if}


