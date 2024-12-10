
<script>
	
	import { client } from "$lib/appwrite";
    import { Button } from "$lib/components/ui/button";
	import CardContent from "$lib/components/ui/card/card-content.svelte";
	import CardHeader from "$lib/components/ui/card/card-header.svelte";
	import Card from "$lib/components/ui/card/card.svelte";
	import Input from "$lib/components/ui/input/input.svelte";
	import { Databases, ID } from "appwrite";


    let name = $state("");
    let message = $state("");


    async function getData(){
        const databases = new Databases(client);
        return databases.listDocuments(
            "67587e6f0008474c03db",
            "67587e9800397aab90a0"
        );
    }

</script>


<div class="p-10 flex flex-col gap-5">

{#await getData()}
    loading
{:then data}
    {#each data.documents as post}
        <Card class="w-min">
            <CardHeader>
                {post.author}
            </CardHeader>
            <CardContent>
                {post.message}
            </CardContent>
            
        </Card>
    {/each}
{/await}




   
    <Input type="text" placeholder="name" class="max-w-xs" bind:value={name} />
    <Input type="text" placeholder="message" class="max-w-xs" bind:value={message} />
    <Button onclick={()=>{
        
        const databases = new Databases(client);

        const promise = databases.createDocument(
            '67587e6f0008474c03db',
            '67587e9800397aab90a0',
            ID.unique(),
            { 
                "author": name,
                "message": message
            }
        );

        promise.then(function (response) {
            console.log(response);
        }, function (error) {
            console.log(error);
        });

    }} class="w-min">Send</Button>
  
</div>
