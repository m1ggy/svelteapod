<script>
  import { onMount } from 'svelte';
  import { onDestroy } from 'svelte/internal';
  import { photoObject } from './stores';
  import { fly, fade } from 'svelte/transition';

  let loading = true;
  let photodets;

  onMount(() => {
    let timer = setTimeout(() => {
      fetch(
        'https://api.nasa.gov/planetary/apod?api_key=FzeCDh8XpjPO9P44Yezch6Q2NkkN8YT0KrFiFyCX'
      )
        .then((res) => res.json())
        .then((data) => {
          photoObject.set(data);
        });
    }, 3000);
    return timer;
  });
  const unsubscribe = photoObject.subscribe((obj) => {
    photodets = obj;
  });
  onDestroy(unsubscribe);
</script>

<div class="wrapper">
  {#if loading}
    <div class="loading-container" out:fade>
      <h2>
        Loading <span class="dot1">.</span><span class="dot2">.</span><span
          class="dot3">.</span
        >
      </h2>
      <div class="photo-container" style="display:none">
        <img
          alt="nasa"
          style="display:none"
          src={photodets.hdurl}
          on:load={() => (loading = false)}
          class="main-photo"
        />
      </div>
    </div>
  {:else}
    <div transition:fly={{ y: 100, duration: 3000 }} class="content-container">
      <div class="photo-container">
        <img alt="nasa" class="main-photo" src={photodets.hdurl} />
      </div>
      <div class="text-container">
        <h1>{photodets.title}</h1>
        <p>
          {new Date(photodets.date).toLocaleString('en', {
            weekday: 'long',
            year: 'numeric',
            month: 'long',
            day: 'numeric',
          })}
        </p>
        {#if photodets.copyright != null}
          <p>{photodets.copyright}</p>
        {/if}

        <p>{photodets.explanation}</p>
      </div>
    </div>
  {/if}
  <span class="star" />
  <span class="star" />
  <span class="star" />
  <span class="star" />
  <span class="star" />
  <span class="star" />
  <span class="star" />
  <span class="star" />
  <span class="star" />
  <span class="star" />
</div>

<style>
  .wrapper {
    color: white;
    width: 100vw;
    height: max-content;
    background-color: black;
    white-space: normal;
    text-align: center;
  }
  .main-photo {
    max-width: 100%;
    max-height: 100vh;
  }
  .content-container {
    color: white;
    height: max-content;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    z-index: 999;
    position: relative;
  }

  .text-container {
    min-height: 100vh;
    text-align: center;
    width: 100vw;
    background-color: #ede3d9;
    color: #141e29;
    height: max-content;
    line-height: 2;
    white-space: normal;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }
  .text-container > p {
    margin-left: 15%;
    margin-right: 15%;
  }
  .loading-container {
    color: white;
    width: 100vw;
    height: 100vh;
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .photo-container {
    width: 100vw;
    height: max-content;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .dot1 {
    animation: 0.5s ease 0.25s infinite alternate-reverse fadein;
  }
  .dot2 {
    animation: 0.5s ease 0.5s infinite alternate-reverse fadein;
  }
  .dot3 {
    animation: 0.5s ease 0.75s infinite alternate-reverse fadein;
  }
  .star {
    position: relative;
    width: 1px;
    height: 2px;
    background-color: white;
    position: fixed;
    z-index: 1;
    animation-name: particleAnimation;
    animation-timing-function: linear;
    animation-iteration-count: infinite;
  }
  .star::before {
    position: absolute;
    display: block;
    content: '';
    width: 25vw;
    right: 1px;
    top: 0;
    z-index: 1;
    height: 1px;
    background: linear-gradient(
      to right,
      rgba(0, 0, 0, 0) 0%,
      rgba(255, 255, 255, 0.4) 100%
    );
  }
  .star:nth-of-type(1) {
    top: 20%;
    animation-duration: 16s;
  }
  .star:nth-of-type(2) {
    top: 40%;
    animation-duration: 5s;
  }
  .star:nth-of-type(3) {
    top: 35%;
    animation-duration: 7s;
  }
  .star:nth-of-type(4) {
    top: 55%;
    animation-duration: 10s;
  }
  .star:nth-of-type(5) {
    top: 80%;
    animation-duration: 15s;
  }
  .star:nth-of-type(6) {
    top: 100%;
    animation-duration: 4s;
  }
  .star:nth-of-type(7) {
    top: 10%;
    animation-duration: 9s;
  }
  .star:nth-of-type(8) {
    top: 25%;
    animation-duration: 3s;
  }
  .star:nth-of-type(9) {
    top: 90%;
    animation-duration: 6s;
  }
  .star:nth-of-type(10) {
    top: 70%;
    animation-duration: 20s;
  }

  @keyframes fadein {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }
  @keyframes particleAnimation {
    from {
      left: -100px;
    }
    to {
      left: calc(100% + 100px);
    }
  }
</style>
