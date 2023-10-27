# Component

### Render Function

```js
Vue.component("app-table", {
    render: function (createElement) {
        return createElement("b-table", {
            props: this.$attrs,
            slots: this.$slots,
            scopedSlots: Object.assign(this.$scopedSlots, {
                "cell(buttons)": function (slotProps) {
                    return createElement({
                        template: "#_AppTableButtons",
                        props: ["detailsShowing", "field", "index", "item", "rowSelected", "unformatted", "value"],
                        methods: {
                            selectRow: slotProps.selectRow,
                            unselectRow: slotProps.unselectRow,
                            toggleDetails: slotProps.toggleDetails
                        }
                    }, {
                        props: slotProps
                    });
                }
            })
        });
    }
});
```
