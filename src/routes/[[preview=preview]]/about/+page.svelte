<script lang='ts'>
    import ContentWidth from "$lib/components/ContentWidth/ContentWidth.svelte";
  import ContactForm from "$lib/components/FullWidth/ContactForm.svelte";
  import ScreenWidthImage from "$lib/components/ScreenWidth/ScreenWidthImage.svelte";
    import { PrismicImage, PrismicRichText } from "@prismicio/svelte";
  import { fade } from "svelte/transition";
  
  
  
      let { data } = $props();
    let content = $state(data.page.data);
  
    let viewportWidth=$state(1024);

    let active = $state(-1);


    let submit = () => {if(form)form.submit()};
    let form:HTMLFormElement|undefined = $state();
    
  </script>
  <svelte:window bind:innerWidth={viewportWidth} />

  <ScreenWidthImage darken flip field={data.page.data.hero_image} class="flex flex-col items-start justify-end py-36 gap-6">
    <h1 class="text-white max-w-screen-md">{content.hero_header||''}</h1>
    <div class="max-w-screen-md text-white whitespace-pre-line flex flex-col gap-2">
      <p>{content.hero_body} </p>
    </div>
  
  </ScreenWidthImage>
  <section class="bg-light py-24">
    <ContentWidth class="text-white mb-10">
        <h2>{content.team_header}</h2>
    </ContentWidth>
    <ContentWidth class="flex md:flex-row justify-end relative h-[600px] gap-10">
        <div class="md:w-1/4 flex flex-col items-start justify-start gap-2 z-20">
            {#if active>-1}
            <button transition:fade={{duration:150}} onclick={()=>active=-1} class="flex flex-col items-start justify-start gap-2 text-white">
                <PrismicImage class="w-full aspect-[3/4]" field={content.team_member[active].headshot||''} />
                <h4>{content.team_member[active].name}</h4>
                <h5>{content.team_member[active].title}</h5>
            
                <div class="mt-8 button-text transition w-36 h-9 border-2 border-white text-white hover:bg-white active:bg-dark active:text-white hover:text-dark flex items-center justify-center">Close</div>
            

            </button>
            {/if}
            
            
        </div>
        {#each content.team_member as p, i}
        <button 
            onclick={()=>{if(active===i){ active=-1}else{active=i}}} 
            class="md:w-1/4 flex flex-col items-start justify-start gap-2">
            <PrismicImage class="w-full aspect-[3/4] object-cover mb-4" field={content.team_member[i].headshot} />
            <h4>{content.team_member[i].name}</h4>
            <h5>{content.team_member[i].title}</h5>


        </button>
        {/each}
        <div class="w-full h-full absolute bg-light flex flex-col justify-start items-end z-10 pointer-events-none transition-opacity duration-150 {active===-1?"opacity-0":""}">
            {#if active>-1}
                <div class="text-white w-2/3" in:fade={{duration:150, delay: 150}} out:fade={{duration:150}}>
                    <PrismicRichText field={content.team_member[active].body||''} />
                </div>
                
            {/if}
        </div>
        
    </ContentWidth>
  </section>
  <section class="bg-white pt-12 -mb-32">
    <ContentWidth class="flex items-start justify-start">
        <h2 class="text-primary">{content.headquarters_header}</h2>
    </ContentWidth>
    <ContentWidth class="flex flex-col mt-16">
        <div class="flex md:flex-row mb-10">
            <PrismicImage field={content.hq_image_one} class="h-[500px] md:w-3/4 object-cover mr-5" />
            <PrismicImage field={content.hq_image_two} class="h-[500px] md:w-1/4 object-cover ml-5"/>
        </div>
        <div class="flex md:flex-row mb-10">
            <PrismicImage field={content.hq_image_three} class="h-[300px] md:w-1/2 object-cover mr-5" />
            <PrismicImage field={content.hq_image_four} class="h-[300px] md:w-1/2 object-cover ml-5"/>
        </div>
    </ContentWidth>

  </section>
  <section class="bg-[#cac2b3] pt-48 pb-16" id="contact">
    <ContentWidth class="flex items-start justify-start">
        <h2 class="text-primary mb-16">Contact Us</h2>
    </ContentWidth>
    <ContentWidth>
        <form class="w-full flex flex-col bg-primary text-white gap-10 p-10" name="contact" method="post" bind:this={form} netlify netlify-honeypot="bot-field">
            <input type="hidden" name="form-name" value="contact" />
            
            <div class="w-full flex flex-row">
                <div class="w-1/12">Name</div>
                <input class="w-11/12 border-[1px] rounded-[3px] text-dark border-light h-10 pl-6" name="name" type="text"/>    
            </div>

            <div class="w-full flex flex-row justify-start">
                <div class="w-1/12">Email</div>
                <input class="w-4/12 border-[1px] rounded-[3px] text-dark border-light h-10 pl-6" name="email" type="email"/>    
                <div class="w-0 md:w-1/6"></div>
                <div class="w-1/12">Phone</div>
                <input class="w-4/12 border-[1px] rounded-[3px] text-dark border-light h-10 pl-6" name="phone" type="phone"/>    
         
            </div>

            <div class="w-full flex flex-row">
                <div class="w-1/12">Message</div>
                <textarea class="w-11/12 border-[1px] rounded-[3px] text-dark border-light h-48 pl-6 pt-2" name="message"></textarea>
            </div>
                <div class="ml-auto mr-20 w-16">
                    <button onclick={submit} class="mt-8 button-text transition w-36 h-9 border-2 border-white text-white hover:bg-white active:bg-dark active:text-white hover:text-dark flex items-center justify-center">Submit</button>
                </div>
                
        </form>
    </ContentWidth>
  </section>
  