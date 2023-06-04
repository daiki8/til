# ion-inputにカスタムディレクティブをあてる

ionic v5

```
  @HostListener("ionFocus", ["$event.target.value"])
  onFocus(value){
    console.log('focus', value);
  }
  @HostListener("ionBlur", ["$event.target.value"])
  onBlur(value){
    console.log('blur', value);
  }
```


## 参考
- https://forum.ionicframework.com/t/ionblur-event-triggered-on-focus-of-an-element/151040
- https://stackoverflow.com/questions/48703377/how-to-access-value-of-the-input-box-surrounded-by-ion-input-directive
- https://ionicframework.com/docs/v5/api/input
- https://swfz.hatenablog.com/entry/2017/08/29/021723

