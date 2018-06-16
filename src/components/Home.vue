
<template>
  <Page class="page">
      <ActionBar>
        <NavigationButton visibility="collapsed" />
      </ActionBar>
    <TabView :selectedIndex="selectedIndex">
      <TabViewItem title="Welcome">
        <welcome-component></welcome-component>
      </TabViewItem>
      <TabViewItem title="Take">
        <take-component></take-component>
      </TabViewItem>
      <TabViewItem title="Trip">
        <trip-component></trip-component>
      </TabViewItem>
      <TabViewItem title="Talk">
        <talk-component></talk-component>
      </TabViewItem>
    </TabView>
  </Page>
</template>

<script>
import WelcomeComponent from "../components/tabs/welcome/WelcomeComponent";
import TakeComponent from "../components/tabs/take/TakeComponent";
import TalkComponent from "../components/tabs/talk/TalkComponent";
import TripComponent from "../components/tabs/trip/TripComponent";

export default {
  components: {
    "welcome-component": WelcomeComponent,
    "take-component": TakeComponent,
    "trip-component": TripComponent,
    "talk-component": TalkComponent
  },
  data( ) {
    return {
      selectedIndex : 0
    }
  }
};
</script>
