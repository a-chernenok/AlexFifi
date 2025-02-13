<script>
    import { Button } from 'flowbite-svelte';
    import { onMount } from 'svelte';
    import { fade, blur, fly } from 'svelte/transition';
    import { goto } from '$app/navigation';
    // Left carousel state
    let leftIndex = 0;
    const leftImages = [
        'images/FC62A1BE-817C-44A0-8EE7-816CDBA75712.JPG',
        'oia-uia.gif',
        'images/IMG_9598.PNG',
        'images/IMG_9598.PNG',
        'images/IMG_2500.jpg',
        'images/40399453-FF8A-4CE4-BA53-E9327EE79384.JPG',
        'images/65F67E25-A663-429C-9F8F-34F5873C6A0C.JPG',
        // Add more images for left carousel
    ];

    function leftNext() {
        leftIndex = (leftIndex + 1) % leftImages.length;
    }

    function leftPrev() {
        leftIndex = (leftIndex - 1 + leftImages.length) % leftImages.length;
    }

    onMount(() => {
        const interval = setInterval(leftNext, 3000); // Change image every 4 seconds for left carousel

        return () => clearInterval(interval); // Clear interval on component unmount
    });

    // Right carousel state
    let rightIndex = 0;
    const rightImages = [
        'images/A31CD955-EA12-496A-9281-CEEB6ACDA146.JPG',
        'images/IMG_9978.jpg',
        'images/IMG_2779.jpg',
        'images/D83245F2-9524-42F3-B0ED-AC2ED5C19443.JPG',
        'images/99C56D7C-6271-4609-A394-BCA84317914A.JPG',
        'images/7A7D92E8-F020-483D-BC29-742406966FB2.JPG',
        // Add more images for right carousel
    ];

    function rightNext() {
        rightIndex = (rightIndex + 1) % rightImages.length;
    }

    function rightPrev() {
        rightIndex = (rightIndex - 1 + rightImages.length) % rightImages.length;
    }

    onMount(() => {
        const interval = setInterval(rightNext, 4000); // Change image every 4 seconds for right carousel

        return () => clearInterval(interval); // Clear interval on component unmount
    });

    let noCount = 0;
    let yesButtonScale = 1; // Initial scale for the Yes button
    let showModal = false; // State to control modal visibility

    function handleNoClick() {
        noCount++;

        if (noCount >= 5) {
            showModal = true; // Show modal when reaching the last click
        } else {
            // Randomly position the No button logic here
            randomPositionNoButton();
            // Grow the Yes button
            yesButtonScale += 1; // Increase scale by 0.1
        }
    }

    function randomPositionNoButton() {
        const noButton = document.getElementById('no-button');
        const buttonWidth = noButton.offsetWidth; // Get button width
        const buttonHeight = noButton.offsetHeight; // Get button height

        const maxX = window.innerWidth - buttonWidth - 800;
        const maxY = window.innerHeight - buttonHeight - 800;

        const randomX = Math.max(0, Math.random() * maxX);
        const randomY = Math.max(0, Math.random() * maxY);
        
        noButton.style.position = 'absolute';
        noButton.style.left = `${randomX}px`;
        noButton.style.top = `${randomY}px`;
    }

    function handleYesClick() {
        goto('/yes'); // Redirect to the yes page
    }

    function handleConfirmNo() {
        goto('/no'); // Redirect to the no page
    }

    function handleCancel() {
        showModal = false; // Close the modal
    }
</script>



<div class="bg-pink-400 h-screen text-gray-700">
    <div class="flex justify-center">
        <h1 class="text-center text-3xl font-bold bg-pink-300 w-1/3 border-4 border-pink-400 rounded-xl">From Alex to Fifi</h1>
    </div>
    <div class="grid grid-cols-12">
        <!-- Left Image Carousel -->
        <div class="col-start-1 col-span-4 flex justify-center items-center mb-65">
            <div class="relative w-full max-w-lg mx-auto">
                <div class="overflow-hidden rounded-lg">
                    {#each leftImages as image, index}
                        <img
                            src={image}
                            alt={`Left Image ${index + 1}`}
                            class="w-full transition-opacity duration-500 ease-in-out max-w-full max-h-180 object-contain border-r-4 border-l-4 border-purple-500 rounded-lg"
                            style="position: absolute; top: 0; left: 0;"
                            transition:blur={{ duration: 500 }}
                            class:opacity-100={leftIndex === index}
                            class:opacity-0={leftIndex !== index}
                        />
                    {/each}
                </div>
            </div>
        </div>

        <!-- Center Content -->
        <div class="col-start-5 col-span-4 text-center mt-28 relative h-64"> <!-- Set a specific height -->
            <img src="oia-uia.gif" alt="Valentine's Image" class="mx-auto rounded-lg"> 
            <h1 class="text-3xl font-bold underline mt-4">Will you be my valentine?</h1>
            <p class="text-purple-400">(please be)</p>
            
            <div class="flex justify-center items-center mt-4 relative">
                <div class="flex space-x-4 mr-15">
                    <div>
                    <button 
                    on:click={handleYesClick} 
                    style="transform: scale({yesButtonScale});" 
                    class="bg-purple-500 text-white py-2 px-4 rounded-lg hover:bg-purple-700 transition duration-300 z-50">
                    Yes!
                </button> 
                </div>
                <div> 
                <button 
                id="no-button" 
                on:click={handleNoClick} 
                class="bg-red-500 text-white py-2 px-4 rounded-lg hover:bg-red-700 transition duration-300 absolute z-50"
                >
                No!
            </button>
        </div>
                </div>
            </div>
        </div>
        {#if showModal}
        <div class="fixed inset-0 flex items-center justify-center bg-black/80 z-50">
            <div class="bg-white p-5 rounded-lg shadow-lg flex flex-col items-center">
                <img src="pokemon-pikachu.gif" alt="Are you sure?" class="w-100 mb-4" />
                <h2 class="text-xl font-bold mb-2 text-center">ARE YOU SURE YOU DONT WANT ME???</h2>
                <p class="mb-4 text-center">change your mind:(</p>
                <div class="flex space-x-4">
                    <button on:click={handleConfirmNo} class="bg-red-500 text-white py-2 px-4 rounded-lg hover:bg-red-700 transition duration-300">
                        NO! I dont want you....
                    </button>
                    <button on:click={handleCancel} class="bg-gray-300 text-black py-2 px-4 rounded-lg hover:bg-gray-400 transition duration-300">
                        Ichanged my mind pookie
                    </button>
                </div>
            </div>
        </div>
    {/if}

        <!-- Right Image Carousel -->
        <div class="col-start-9 col-span-4 flex justify-center items-center mb-65">
            <div class="relative w-full max-w-lg mx-auto">
                <div class="overflow-hidden rounded-lg">
                    {#each rightImages as image, index}
                        <img
                            src={image}
                            alt={`Right Image ${index + 1}`}
                            class="w-full transition-opacity duration-500 ease-in-out max-w-full max-h-180 object-contain border-r-4 border-l-4 border-purple-500 rounded-lg"
                            style="position: absolute; top: 0; left: 0;"
                            transition:fade={{ duration: 500 }}
                            class:opacity-100={rightIndex === index}
                            class:opacity-0={rightIndex !== index}
                        />
                    {/each}
                </div>
            </div>
        </div>
    </div>
</div>