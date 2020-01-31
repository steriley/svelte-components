<script>
  import Countdown from './Countdown.svelte';

  const i18n = {
    ESP: {
      days: 'dias',
      hours: 'horas',
      minutes: 'minutos',
      seconds: 'segundos',
    },
    FRA: {
      days: 'journées',
      hours: 'heures',
      minutes: 'min',
      seconds: 'secondes',
    },
    DEU: {
      days: 'Tage',
      hours: 'Std',
      minutes: 'Minuten',
      seconds: 'Sekunden',
    },
  };

  let hasExpired = false;
  const onExpire = () => hasExpired = !hasExpired;
</script>

{#if hasExpired}
  <h1 class="expired">The End!</h1>
{:else}
  <Countdown date="31 January, 2020 23:00" on:expired={onExpire} />
{/if}

<style>
  .expired {
    text-align: center;
  }
</style>
