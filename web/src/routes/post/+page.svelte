<script lang="ts">
  import categories from "@/categories";
  import { Button } from "@/components/ui/button";
  import { Input } from "@/components/ui/input";
  import { Select, SelectContent, SelectGroup, SelectInput, SelectItem, SelectLabel, SelectTrigger, SelectValue } from "@/components/ui/select";
  import Textarea from "@/components/ui/textarea/textarea.svelte";
  import { Image } from 'lucide-svelte'

  let inputEl: HTMLInputElement

  let files: FileList

  let file_urls: string[] = []

  function handleChange(){

      const reader = new FileReader()
    
      reader.onload = e => {
          file_urls = file_urls.concat([e.target?.result as string])
      }
    
    //   for (const file of files) {
    //       reader.readAsDataURL(file)
    //   }
      reader.readAsDataURL(files[0])
  }
    
</script>

<div class="h-full w-full flex items-center justify-center bg-gray-50">
    
    <form method="post" class="border shadow-lg rounded-lg flex flex-col gap-4 items-center p-2 w-full md:w-96" enctype="multipart/form-data">
        <input bind:files on:change={handleChange} bind:this={inputEl} id="select" name="file" type="file" class="hidden">

        {#if files?.length < 1 || !files} 
            <Image class="opacity-30" size='50'/>
            <Button type="button" on:click={() => inputEl?.click()} variant="secondary">Choose Some images</Button>

            {:else}
            <div class="w-full h-[300px] rounded overflow-hidden">
                <img src="{file_urls[0]}" alt="file" class="h-full w-full object-cover">
            </div>

            <Input name="price" class="w-full" type="number" placeholder="Price - NGN" required />

            <Input name="name" class="w-full" type="text" placeholder="Product name" required />

            <Textarea placeholder="Product Description" name="description" class="w-full resize-none" required/>
            
            <Select>
              <SelectTrigger class="w-full">
                <SelectValue placeholder="Pick a Category"/>
              </SelectTrigger>
              <SelectContent>
                <SelectGroup>
                  <SelectLabel>Categories</SelectLabel>
                  {#each categories as category}
                    <SelectItem value={category} label={category}
                      >{category}</SelectItem
                    >
                  {/each}
                </SelectGroup>
              </SelectContent>
              <SelectInput name="category" required/>
            </Select>

            <Button class="w-full" type="submit">Post</Button>
        {/if}
    </form>

</div>