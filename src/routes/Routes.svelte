<script>
  import page from "page";

  // Pages
  import Index from "./Index.svelte";
  import UserActivity from "./UserActivity.svelte";
  import UsageBehavior from "./UsageBehavior.svelte";
  import Hardware from "./Hardware.svelte";

  let component;
  let params;

  function setComponent(c) {
    return function setComponentInner({ params: p }) {
      component = c;
      params = p;
    };
  }

  page("/", setComponent(Index));
  page("/dashboard/user-activity", setComponent(UserActivity));
  page("/dashboard/usage-behavior", setComponent(UsageBehavior));
  page("/dashboard/hardware", setComponent(Hardware));
  page({ hashbang: true });
</script>

<svelte:component this={component} bind:params />
