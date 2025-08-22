---
layout: post
title: About Michelle Ji
permalink: /about/
comments: true
---

## Learn About Me!

Here are some places I have lived.

<style>
    /* Style looks pretty compact, 
       - grid-container and grid-item are referenced the code 
    */
    .grid-container {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); /* Dynamic columns */
        gap: 10px;
    }
    .grid-item {
        text-align: center;
    }
    .grid-item img {
        width: 100%;
        height: 100px; /* Fixed height for uniformity */
        object-fit: contain; /* Ensure the image fits within the fixed height */
    }
    .grid-item p {
        margin: 5px 0; /* Add some margin for spacing */
    }

    .image-gallery {
        display: flex;
        flex-wrap: nowrap;
        overflow-x: auto;
        gap: 10px;
        }

    .image-gallery img {
        max-height: 150px;
        object-fit: cover;
        border-radius: 5px;
    }
</style>

<!-- This grid_container class is used by CSS styling and the id is used by JavaScript connection -->
<div class="grid-container" id="grid_container">
    <!-- content will be added here by JavaScript -->
</div>

<script>
    // 1. Make a connection to the HTML container defined in the HTML div
    var container = document.getElementById("grid_container"); // This container connects to the HTML div

    // 2. Define a JavaScript object for our http source and our data rows for the Living in the World grid
    var http_source = "https://upload.wikimedia.org/wikipedia/commons/";
    var living_in_the_world = [
        {"flag": "0/01/Flag_of_California.svg", "description": "California - Forever"},
        {"flag": "f/fa/Flag_of_the_People%27s_Republic_of_China.svg", "description": "China - Once Every Year"},
    ];

    // 3a. Consider how to update style count for size of container
    // The grid-template-columns has been defined as dynamic with auto-fill and minmax

    // 3b. Build grid items inside of our container for each row of data
    for (const location of living_in_the_world) {
        // Create a "div" with "class grid-item" for each row
        var gridItem = document.createElement("div");
        gridItem.className = "grid-item";  // This class name connects the gridItem to the CSS style elements
        // Add "img" HTML tag for the flag
        var img = document.createElement("img");
        img.src = http_source + location.flag; // concatenate the source and flag
        img.alt = location.flag + " Flag"; // add alt text for accessibility

        // Add "p" HTML tag for the description
        var description = document.createElement("p");
        description.textContent = location.description; // extract the description

        // Add "p" HTML tag for the greeting
        var greeting = document.createElement("p");
        greeting.textContent = location.greeting;  // extract the greeting

        // Append img and p HTML tags to the grid item DIV
        gridItem.appendChild(img);
        gridItem.appendChild(description);
        gridItem.appendChild(greeting);

        // Append the grid item DIV to the container DIV
        container.appendChild(gridItem);
    }
</script>
### Birthday

🎂 My birthday is June 21st, 2010

### Journey through Life

Here are the schools I've went/going to in California:

- 👶 Monterey Ridge Elementary School, 2015-2021
- 👩‍🦰 Oak Valley Middle School, 2021-2024
- 👵 Del Norte High School, 2024-current

### Culture, Family, and Fun

Me, My Culture, and My Family:

- My parents are both from China, but I was born here, so I am American Chinese
- My brother, who was born in New York, is 21 years old, and goes to University of Washington
- My first language is English, but I am also mostly fluent in Mandarin 
- 🩰 I main sport/hobby is competitve dance 
    - I have been dancing since I was 4 years old
    - 💃 I am on the Del Norte Dance Team 
    - My favorite styles are ballet, hip hop, and contemporary 
    - I currently dance at Inspired Movement Dance where I focus on ballet and contemporary technique
- Some side hobbies/activites: crochet, watching TV shows or movies, piano, electric guitar, listening to music
    - 📺 Fav TV shows or movies: Scream, The Texas Chainsaw Massacre, Nezha 1 & 2, The Glory, You (in general I love horrors)
    - 🎙️ Fav artist: The Weeknd 
    - 🎶 Fav songs: Lonely Star- The Weeknd, Double Fantasy- The Weeknd, Oxytocin- Billie Eilish, Good God- Korn, IN MY MOUTH- Black Dresses, etc.
- The pictures below are about me and what I value most in life

<comment>
Gallery of Pics, scroll to the right for more ...
</comment>
<div class="image-gallery">
  <img src="{{site.baseurl}}/images/about/missionary.jpg" alt="Image 1">
  <img src="{{site.baseurl}}/images/about/john_tamara.jpg" alt="Image 2">
  <img src="{{site.baseurl}}/images/about/tamara_fam.jpg" alt="Image 3">
  <img src="{{site.baseurl}}/images/about/surf.jpg" alt="Image 4">
  <img src="{{site.baseurl}}/images/about/john_lora.jpg" alt="Image 5">
  <img src="{{site.baseurl}}/images/about/lora_fam.jpg" alt="Image 6">
  <img src="{{site.baseurl}}/images/about/lora_fam2.jpg" alt="Image 7">
  <img src="{{site.baseurl}}/images/about/pj_party.jpg" alt="Image 8">
  <img src="{{site.baseurl}}/images/about/trent_family.png" alt="Image 9">
  <img src="{{site.baseurl}}/images/about/claire.jpg" alt="Image 10">
  <img src="{{site.baseurl}}/images/about/grandkids.jpg" alt="Image 11">
  <img src="{{site.baseurl}}/images/about/farm.jpg" alt="Image 12">
</div>
