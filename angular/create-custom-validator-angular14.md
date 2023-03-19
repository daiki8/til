# カスタムバリデーションを作る

クラス作成
```shell
ng g class pages/validators/product-code-product-name-validator
```

バリデーションを実装
```typescript
const PRODUCT_CODE = 'productCode';
const PRODUCT_NAME = 'productName';
export const ProductCodeProductNameValidator: ValidatorFn = (control: FormGroup): ValidationErrors | null => {
  const productCode = control.get(PRODUCT_CODE).value;
  const productName = control.get(PRODUCT_NAME).value;

  if (productCode && productName) {
    control.get(PRODUCT_CODE).setErrors(null);
    return null;
  }

  const validateError = { productNotExistError: true };
  control.get(PRODUCT_CODE).setErrors(validateError);
  return validateError;
};
```

以下について
validatorFn
FormGroup
ValidationErrors

これらは、Angularフレームワークでフォームのバリデーションを実装するために使用される重要な概念です。

validatorFn:
validatorFnは、Angularで使用される関数で、フォームのバリデーションを実行するために使用されます。この関数は、FormControlオブジェクトの値が有効か無効かを判断し、適切なエラーメッセージを返します。例えば、パスワードが8文字以上であるかどうかを確認するために使用できます。

FormGroup:
FormGroupは、Angularで使用されるフォームグループを表すクラスです。FormGroupは、複数のフォームコントロールをグループ化するために使用され、フォームグループのバリデーションを実行するためにも使用されます。例えば、ユーザー登録フォームにおいて、名前、メールアドレス、パスワードなどのフォームコントロールをグループ化するために使用できます。

ValidationErrors:
ValidationErrorsは、Angularで使用されるクラスで、フォームのバリデーション時に発生したエラーを表します。例えば、パスワードが8文字未満である場合には、"passwordTooShort"というエラーメッセージがValidationErrorsオブジェクトに格納されます。ValidationErrorsは、フォームのバリデーション時に発生したエラーをキャッチし、それに応じたエラーメッセージをユーザーに表示するために使用されます。


バリデーションエラーが発生した場合、mat-errorタグを表示
```html
<mat-error class="validation-error" *ngIf="productCode.hasError('productNotExistError')">
  {{'stockRegisteringPage.validationError.productNotExistError'|translate}}
</mat-error>
```

FormGroupに作成したカスタムバリデーションを追加
```typescript
registeringForm = this.formBuilder.group(
  {
    productCode: this.productCode,
    productName: this.productName,
    productGenre: this.productGenre,
    productSizeStandard: this.productSizeStandard,
    productStockQuantity: this.productStockQuantity,
    addProductStockQuantity: this.addProductStockQuantity,
    productImage: this.productImage
  },
  ProductCodeProductNameValidator
);
```

## 参考
https://www.udemy.com/course/angular_hands_on_unit_e2e_test/learn/lecture/25891882#notes
