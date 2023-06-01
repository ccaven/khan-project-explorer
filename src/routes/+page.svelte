<script lang="ts">
    import { onMount } from "svelte";
    import projects from "./projects.json";

    let images: HTMLImageElement[] = [];

    function onMouseMove(event: MouseEvent) {

        for (let i = 0; i < images.length; i ++) {
            
            let image = images[i];
            
            let mouseX = event.x;
            let mouseY = event.y;

            let boundingRect = image.getBoundingClientRect();

            let centerX = boundingRect.x + boundingRect.width / 2;
            let centerY = boundingRect.y + boundingRect.height / 2;

            let distance = Math.sqrt(
                Math.pow(mouseX - centerX, 2) +
                Math.pow(mouseY - centerY, 2)
            );

            let threshold = 100;
            let blendSize = 200;
            
            if (distance < 50) {
                
                image.style.transform = "scale(120%)";
                image.style.zIndex = "1";

            } else {
                
                image.style.transform = "";
                image.style.zIndex = "0";
                
            }

            if (distance > threshold) {

                let k = (distance - threshold) / blendSize;

                if (k > 1) k = 1;

                image.style.borderRadius = Math.round(50 * k) + "px";
                image.style.filter = "blur(" + Math.round(5 * k) + "px";

            } else {

                image.style.borderRadius = "0px";
                image.style.filter = "";
            
            }

        
        }

    }


</script>

<div on:mousemove={onMouseMove}>

    {#each projects as project, i}

        <a href={project.href} target="_blank">
            
            <img 
                src={project.src} 
                alt="" 
                width=100

                bind:this={images[i]}
            >

        </a>
        
    {/each}

</div>

<style>

    :global(body) {
        padding: 0;
        margin: 0;
    }

    div {
        margin: 0;
        padding: 0;
        border: none;
        background-color: rgb(14, 14, 14);
    }

    a {
        text-decoration: none;
    }

    img {
        padding: 0;
        margin: 0;
        transition: 0.2s ease;
        position: relative;
    }

</style>

