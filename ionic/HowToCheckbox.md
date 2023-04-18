# Ionic How To Get Ion Checkbox with Vue

```
<ion-item v-for="item in items" :key="item.id">
  <ion-label>{{ item.name }}</ion-label>
  <ion-checkbox v-model="item.checked"></ion-checkbox>
</ion-item>

interface Item {
    id: string;
    name: string;
    checked: boolean;
}

const boxFileList: Ref<Array<Item>> = ref([])
```

https://stackoverflow.com/questions/50399526/how-to-get-ion-checkbox-multiple-select-value-in-ionic-3
