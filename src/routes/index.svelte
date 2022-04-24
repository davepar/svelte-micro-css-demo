<script context="module" lang="ts">
  /** @type {import('customers.Load} */
  export async function load({ params, fetch, session, stuff }) {
    const response = await fetch('./customers');
    return {
      status: response.status,
      props: {
        data: response.ok && (await response.json()),
      },
    };
  }
</script>

<script lang="ts">
  import { fade } from 'svelte/transition';
  import type { Customer } from './customers';
  import { Frequency, Region, FREQUENCY_OPTIONS, REGION_OPTIONS } from './customers';

  export let data: {
    customers: Customer[];
  };

  let dialogOpen = false;

  function openDialog() {
    dialogOpen = true;
  }

  function closeDialog() {
    dialogOpen = false;
  }

  function saveCustomer() {
    data.customers.push(newCustomer);
    data = { ...data };
    dialogOpen = false;
  }

  const newCustomer = {
    name: '',
    region: null,
    frequency: null,
    newsletter: false,
  };
</script>

<div class="container">
  <div class="content">
    <h1>Sample page for Svelte with a micro CSS framework</h1>
    <p>Visit <a href="https://kit.svelte.dev">web.site.here</a> for more info</p>
  </div>
  <div class="block">
    <button type="button" class="button is-primary" on:click={openDialog}> Add Customer </button>
  </div>

  <table class="table is-bordered">
    <thead>
      <tr>
        <th>Name</th>
        <th>Region</th>
        <th>Frequency</th>
        <th>Newsletter</th>
      </tr>
    </thead>
    <tbody>
      {#each data.customers as customer}
        <tr>
          <td>{customer.name}</td>
          <td>{Region[customer.region]}</td>
          <td>{Frequency[customer.frequency]}</td>
          <td class="has-text-centered">{@html customer.newsletter ? '&#10003;' : ''}</td>
        </tr>
      {/each}
    </tbody>
  </table>
</div>

{#if dialogOpen}
  <div
    class="modal is-active"
    role="dialog"
    aria-modal="true"
    aria-labelledby="add-customer-title"
    transition:fade={{ duration: 100 }}
  >
    <div class="modal-background" on:click={closeDialog} />
    <div class="modal-card">
      <header class="modal-card-head">
        <p class="modal-card-title" id="add-customer-title">Add customer</p>
        <button class="delete" aria-label="close" on:click={closeDialog} />
      </header>
      <section class="modal-card-body">
        <div class="field">
          <label class="label" for="name">Name</label>
          <div class="control">
            <input class="input" type="text" id="name" bind:value={newCustomer.name} />
          </div>
          <p class="help">Customer's full name.</p>
        </div>

        <div class="field">
          <label class="label" for="region">Region</label>
          <div class="control">
            <div class="select">
              <select bind:value={newCustomer.region} id="region">
                {#each REGION_OPTIONS as { label, value }}
                  <option {value}>{label}</option>
                {/each}
              </select>
            </div>
          </div>
        </div>

        <div class="field">
          <div class="control">
            <fieldset>
              <legend class="label">Frequency</legend>
              {#each FREQUENCY_OPTIONS as { label, value }}
                <label class="radio">
                  <input type="radio" name="frequency" bind:group={newCustomer.frequency} {value} />
                  {label}
                </label>
              {/each}
            </fieldset>
          </div>
        </div>

        <div class="field">
          <div class="control">
            <label class="checkbox">
              <input type="checkbox" bind:value={newCustomer.newsletter} />
              Newsletter
            </label>
          </div>
        </div>
      </section>
      <footer class="modal-card-foot">
        <button class="button is-success" on:click={saveCustomer}>Save changes</button>
        <button class="button" on:click={closeDialog}>Cancel</button>
      </footer>
    </div>
  </div>
{/if}
