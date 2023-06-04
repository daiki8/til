# ion-inputにカスタムディレクティブをあてる

```
  @HostListener("ionFocus")
  onFocus(value){
    console.log('focus', this.elementRef.nativeElement.getElementsByTagName('input'));
  }
  @HostListener("ionBlur")
  onBlur(value){
    console.log('blur', this.elementRef.nativeElement.getElementsByTagName('input'));
  }
```


## 参考
https://forum.ionicframework.com/t/ionblur-event-triggered-on-focus-of-an-element/151040
https://stackoverflow.com/questions/48703377/how-to-access-value-of-the-input-box-surrounded-by-ion-input-directive
