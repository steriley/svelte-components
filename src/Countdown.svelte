<script>
  import { onMount, createEventDispatcher } from 'svelte';

  const emit = createEventDispatcher();

  export let date = ''; // format '25 July, 2020 14:45'
  export let seperator = false;
  export let content = {
    days: 'days',
    hours: 'hours',
    minutes: 'mins',
    seconds: 'secs',
  }

  let interval = false;

  onMount(() => init());

  const init = () => {
    if (!expired) {
      emit('active', true);
      interval = setInterval(() => {
        now = Math.trunc(new Date().getTime() / 1000);
        let secondsLeft = countdownDate - now;

        emit('tick', { secondsLeft });

        if (!secondsLeft) {
          hasExpired();
        }
      }, 1000);

      return true;
    }

    return hasExpired();
  };

  const hasExpired = () => {
    emit('expired', date);
    return clearInterval(interval);
  };

  const twoDigits = (value) => {
    let str = value;

    if (str.toString().length <= 1) {
      str = `0${value.toString()}`;
    }

    return str
      .toString()
      .split('')
      .map((x) => `<span>${x}</span>`)
      .join('');
  };

  $: now = Math.trunc(new Date().getTime() / 1000);
  $: countdownDate = Math.trunc(Date.parse(date) / 1000) || 0;
  $: seconds = twoDigits((countdownDate - now) % 60);
  $: minutes = twoDigits(Math.trunc((countdownDate - now) / 60) % 60);
  $: hours = twoDigits(Math.trunc((countdownDate - now) / 60 / 60) % 24);
  $: days = twoDigits(Math.trunc((countdownDate - now) / 60 / 60 / 24));
  $: expired = countdownDate - now >= 0 === false;
</script>


{#if !expired}
  <div class="countdown">
    <div class="segment">
      <div class="number">{@html days}</div>
      <div class="label">{content.days}</div>
    </div>

    {#if seperator}
      <div class="seperator">:</div>
    {/if}

    <div class="segment">
      <div class="number">{@html hours}</div>
      <div class="label">{content.hours}</div>
    </div>

    {#if seperator}
      <div class="seperator">:</div>
    {/if}

    <div class="segment">
      <div class="number">{@html minutes}</div>
      <div class="label">{content.minutes}</div>
    </div>

    {#if seperator}
      <div class="seperator">:</div>
    {/if}

    <div class="segment">
      <div class="number">{@html seconds}</div>
      <div class="label">{content.seconds}</div>
    </div>
  </div>
{/if}


<style>
.countdown {
  align-content: center;
  align-items: center;
  display: flex;
  justify-content: center;
}

.segment {
  padding: 1rem;
  text-align: center;
}
</style>
