<script lang="ts">
  import { onMount, onDestroy } from 'svelte';
  import confetti from 'canvas-confetti';
  import { gsap } from 'gsap';

  const targetDate = new Date('2025-05-24T00:00:00');
  let days = 0;
  let hours = 0;
  let minutes = 0;
  let seconds = 0;
  let isExpired = false;
  let interval: number;
  let wordIndex = 0;
  const words = ['amor', 'belleza', 'culona', 'belleza agropecuaria', 'oriental holguinera', 'racista favorita', 'diosa'];
  let word = words[wordIndex];

  function calculateTimeLeft() {
    const now = new Date();
    const difference = targetDate.getTime() - now.getTime();

    if (difference <= 0) {
      isExpired = true;
      clearInterval(interval);
      celebrateBirthday();
      return;
    }

    const newDays = Math.floor(difference / (1000 * 60 * 60 * 24));
    const newHours = Math.floor((difference % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
    const newMinutes = Math.floor((difference % (1000 * 60 * 60)) / (1000 * 60));
    const newSeconds = Math.floor((difference % (1000 * 60)) / 1000);

    if (newSeconds !== seconds) {
      animateNumber('.seconds', seconds, newSeconds);
    }
    if (newMinutes !== minutes) {
      animateNumber('.minutes', minutes, newMinutes);
    }
    if (newHours !== hours) {
      animateNumber('.hours', hours, newHours);
    }
    if (newDays !== days) {
      animateNumber('.days', days, newDays);
    }

    days = newDays;
    hours = newHours;
    minutes = newMinutes;
    seconds = newSeconds;
  }

  function animateNumber(selector: string, from: number, to: number) {
    gsap.to(document.querySelector(selector), {
      textContent: to,
      duration: 0.5,
      snap: { textContent: 1 },
      ease: "elastic.out(1, 0.5)"
    });
  }

  function celebrateBirthday() {
    const duration = 15 * 1000;
    const animationEnd = Date.now() + duration;
    const defaults = { startVelocity: 30, spread: 360, ticks: 60, zIndex: 0 };

    function randomInRange(min: number, max: number) {
      return Math.random() * (max - min) + min;
    }

    const interval = setInterval(function() {
      const timeLeft = animationEnd - Date.now();

      if (timeLeft <= 0) {
        return clearInterval(interval);
      }

      const particleCount = 50 * (timeLeft / duration);

      confetti({
        ...defaults,
        particleCount,
        origin: { x: randomInRange(0.1, 0.3), y: Math.random() - 0.2 },
        colors: ['#ff69b4', '#ffb6c1', '#ffd700', '#ff8fab']
      });
      confetti({
        ...defaults,
        particleCount,
        origin: { x: randomInRange(0.7, 0.9), y: Math.random() - 0.2 },
        colors: ['#ff69b4', '#ffb6c1', '#ffd700', '#ff8fab']
      });
    }, 250);

    if ('vibrate' in navigator) {
      navigator.vibrate([200, 100, 200, 100, 400]);
    }

    createSparkles();
    animateCat();
  }

  function createHeart() {
    const heart = document.createElement('div');
    heart.className = 'heart';
    heart.style.left = Math.random() * 100 + 'vw';
    heart.style.animationDuration = Math.random() * 2 + 3 + 's';
    document.querySelector('.hearts')?.appendChild(heart);
    setTimeout(() => heart.remove(), 5000);
  }

  function createSparkles() {
    const sparklesContainer = document.createElement('div');
    sparklesContainer.className = 'sparkles';
    document.body.appendChild(sparklesContainer);

    for (let i = 0; i < 50; i++) {
      const sparkle = document.createElement('div');
      sparkle.className = 'sparkle';
      sparkle.style.left = Math.random() * 100 + 'vw';
      sparkle.style.top = Math.random() * 100 + 'vh';
      sparkle.style.animationDelay = Math.random() * 2 + 's';
      sparklesContainer.appendChild(sparkle);
    }
  }

  function animateCat() {
    gsap.to('.cat-container', {
      rotation: 360,
      duration: 1,
      ease: "power2.out"
    });
  }

  function handleDoubleClick() {
    window.open('https://eduardoprofe666.github.io', '_blank');
  }

  function changeWord() {
    const target = document.querySelector('.highlight');
    gsap.to(target, {
      opacity: 0,
      duration: 0.3,
      ease: "power2.out",
      onComplete: () => {
        wordIndex = (wordIndex + 1) % words.length;
        word = words[wordIndex];
        gsap.to(target, {
          opacity: 1,
          duration: 0.3,
          ease: "power2.out"
        });
      }
    });
  }

  onMount(() => {
    calculateTimeLeft();
    interval = setInterval(calculateTimeLeft, 1000);
    setInterval(createHeart, 300);

    gsap.to('.cat-face', {
      scale: 1.05,
      duration: 2,
      repeat: -1,
      yoyo: true,
      ease: "power1.inOut"
    });

    gsap.to('.cat-eye', {
      scaleY: 0.3,
      duration: 0.1,
      repeat: -1,
      repeatDelay: 5,
      yoyo: true
    });

    gsap.to('.cat-whisker', {
      rotation: "+=10",
      duration: 1.5,
      repeat: -1,
      yoyo: true,
      ease: "none",
      stagger: {
        each: 0.2,
        from: "random"
      }
    });

    gsap.to('.cat-ear', {
      rotation: "+=5",
      duration: 2,
      repeat: -1,
      yoyo: true,
      ease: "power1.inOut"
    });

    gsap.to('.cat-nose', {
      scale: 1.1,
      duration: 1,
      repeat: -1,
      yoyo: true,
      ease: "power1.inOut"
    });
  });

  onDestroy(() => {
    clearInterval(interval);
  });
</script>

<main>
  <div class="hearts"></div>

  <div class="cat-container" role="button" tabindex="0" on:dblclick={handleDoubleClick} style="cursor: pointer;">
    <div class="cat-ear left">
      <div class="cat-ear-inner"></div>
    </div>
    <div class="cat-ear right">
      <div class="cat-ear-inner"></div>
    </div>
    <div class="cat-face">
      <div class="cat-eyes">
        <div class="cat-eye"></div>
        <div class="cat-eye"></div>
      </div>
      <div class="cat-nose"></div>
      <div class="cat-mouth"></div>
      <div class="cat-whiskers-container">
        <div class="cat-whisker left1"></div>
        <div class="cat-whisker left2"></div>
        <div class="cat-whisker left3"></div>
        <div class="cat-whisker right1"></div>
        <div class="cat-whisker right2"></div>
        <div class="cat-whisker right3"></div>
      </div>
    </div>
  </div>

  {#if !isExpired}
    <h1 class="birthday-message">Esperanding el cumpleaños de mi <button class="highlight" on:click={changeWord}>{word}</button> ❤️</h1>
    <div class="countdown-container">
      <div class="countdown-box">
        <p class="countdown-number days">{days}</p>
        <p class="countdown-label">Días</p>
      </div>
      <div class="countdown-box">
        <p class="countdown-number hours">{hours}</p>
        <p class="countdown-label">Horas</p>
      </div>
      <div class="countdown-box">
        <p class="countdown-number minutes">{minutes}</p>
        <p class="countdown-label">Mins</p>
      </div>
      <div class="countdown-box">
        <p class="countdown-number seconds">{seconds}</p>
        <p class="countdown-label">Segs</p>
      </div>
    </div>
  {:else}
    <h1 class="birthday-message">¡Feliz Cumpleaños mi <button class="highlight" on:click={changeWord}>{word}</button>! 🎂✨</h1>
  {/if}
</main>