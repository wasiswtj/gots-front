<template>
  <nb-container :style="{ backgroundColor: '#fff' }">
    <nb-header>
      <nb-left>
        <nb-button
          transparent
          :onPress="() => this.props.navigation.openDrawer()"
        >
          <nb-icon name="menu" />
        </nb-button>
      </nb-left>
      <nb-body>
        <nb-title>Picker</nb-title>
      </nb-body>
      <nb-right />
    </nb-header>
    <nb-content padder>
      <nb-list>
        <nb-list-item
          v-for="listItem in listItemArr"
          :key="listItem.route"
          button
          :onPress="() => this.props.navigation.navigate(listItem.route)"
        >
          <nb-left>
            <nb-text>
              {{ listItem.text }}
            </nb-text>
          </nb-left>
          <nb-right>
            <nb-icon name="arrow-forward" class="body-icon-color" />
          </nb-right>
        </nb-list-item>
      </nb-list>
    </nb-content>
  </nb-container>
</template>

<script>
export default {
  data() {
    return {
      listItemArr: [
        {
          route: "RegularPicker",
          text: "Regular"
        },
        {
          route: "PickerWithIcon",
          text: "Picker with Icon"
        },
        {
          route: "PlaceholderPicker",
          text: "Placeholder"
        },
        {
          route: "PlaceholderPickerNote",
          text: "Placeholder (without note)"
        },
        {
          route: "PickerTextItemText",
          text: "Picker text and item text style"
        },
        {
          route: "BackButtonPicker",
          text: "Custom Back Button"
        },
        {
          route: "CustomHeaderPicker",
          text: "Custom Header"
        },
        {
          route: "HeaderPicker",
          text: "Custom Header Text"
        },
        {
          route: "HeaderStylePicker",
          text: "Custom Header Style"
        }
      ]
    };
  }
};
</script>

<style>
.body-icon-color {
  color: #999;
}
</style>
